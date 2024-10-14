### Running nextjs with pm2:

**Install PM2 Globally**
------------------------

**PM2** is a production-grade process manager for Node.js applications, offering features like automatic restarts, load balancing, and more.

1.  **Install PM2 Using NPM:**

    bash

    Copy code

    `npm install -g pm2`

    > **Note:** Using `sudo` is generally not recommended with NPM and PM2. Ensure your user has the necessary permissions.

2.  **Verify PM2 Installation:**

    bash

    Copy code

    `pm2 -v`

    You should see the PM2 version number, e.g., `5.3.0`.

* * * * *

**Step 8: Start Your Next.js App with PM2**
-------------------------------------------

Launch your Next.js application using PM2 to manage the process.

1.  **Start the Application:**

    Depending on your project's setup, you might start the app using `npm start`, `yarn start`, or directly using `next start`.

    -   **Using NPM:**

        `pm2 start npm --name "nextjs-app" -- start`

    -   **Using Yarn:**

        `pm2 start yarn --name "nextjs-app" -- start`

    -   **Directly with Next.js:**

        `pm2 start node --name "nextjs-app" -- node_modules/.bin/next start`

    **Explanation:**

    -   `pm2 start`: Command to start a new process.
    -   `npm` or `yarn` or `node`: The executable to run.
    -   `--name "nextjs-app"`: Assigns a name to the PM2 process for easy identification.
    -   `-- start`: The script to execute, as defined in your `package.json`.
2.  **Verify the Process is Running:**

    `pm2 list`

    **Expected Output:**

    `┌─────┬─────────────┬──────┬────────┬───┬─────┬─────────┐
    │ id  │ name        │ mode │ status │ ↺ │ cpu │ memory  │
    ├─────┼─────────────┼──────┼────────┼───┼─────┼─────────┤
    │ 0   │ nextjs-app  │ fork │ online │ 0 │ 0%  │ 100MB   │
    └─────┴─────────────┴──────┴────────┴───┴─────┴─────────┘`

3.  **Monitor Logs (Optional):**

    To view real-time logs of your application:

    `pm2 logs nextjs-app`

    Press `Ctrl + C` to exit the log view.

* * * * *

**Step 2: Configure PM2 to Start on System Boot**
-------------------------------------------------

Ensure your Next.js application automatically restarts after a server reboot.

1.  **Generate PM2 Startup Script:**

    PM2 can generate and configure a startup script tailored to your system's init system.


    `pm2 startup`

    **Example Output:**


    `[PM2] Init System found: systemd
    [PM2] To setup the Startup Script, copy/paste the following command:
    sudo env PATH=$PATH:/home/username/.nvm/versions/node/v20.x.x/bin pm2 startup systemd -u username --hp /home/username`

2.  **Execute the Suggested Command:**

    Copy and run the command provided in the output. For example:

    `sudo env PATH=$PATH:/home/siam923/.nvm/versions/node/v20.x.x/bin pm2 startup systemd -u siam923 --hp /home/siam923`

    > **Note:** Replace `/home/siam923/.nvm/...` and `/home/siam923` with your actual NVM path and home directory.

3.  **Save the PM2 Process List:**

    This ensures that PM2 remembers the processes to restart on boot.


    `pm2 save`

    **Expected Output:**


    `[PM2] Saving current process list...
    [PM2] Successfully saved in /home/siam923/.pm2/dump.pm2`

4.  **Verify PM2 Startup Configuration:**

    Reboot your system to test if PM2 restarts your application automatically.


    `sudo reboot`

    After the system restarts, reconnect via SSH and check the PM2 process list:

    `pm2 list`

    Your Next.js application should be listed as `online`.

* * * * *

**Step : Verify Your Application is Running**
-----------------------------------------------

Ensure that your Next.js application is accessible and running as expected.

1.  **Check Application Status:**

    `pm2 status nextjs-app`

**Additional Considerations**
-----------------------------

### **1\. Using Environment Variables**

If your Next.js application relies on environment variables:

-   **Create a `.env` File:**

    bash

    Copy code

    `nano .env`

    **Example `.env` Content:**

    env

    Copy code

    `DATABASE_URL=your_database_url
    API_KEY=your_api_key`

-   **Load Environment Variables in PM2:**

    Modify the PM2 start command to include the `.env` file.

    bash

    Copy code

    `pm2 start npm --name "nextjs-app" -- start --env-file .env`

    > **Note:** Ensure your application is configured to load environment variables from the `.env` file.

### **2\. Setting Up a Reverse Proxy (Optional but Recommended)**

For production environments, it's advisable to set up a reverse proxy (e.g., **Nginx**) to handle HTTP/HTTPS traffic, manage SSL certificates, and improve security.

1.  **Install Nginx:**

    bash

    Copy code

    `sudo apt install nginx -y`

2.  **Configure Nginx as a Reverse Proxy:**

    bash

    Copy code

    `sudo nano /etc/nginx/sites-available/nextjs-app`

    **Example Nginx Configuration:**

    nginx

    `server {
        listen 80;
        server_name your_domain.com www.your_domain.com;

        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }`

    > **Replace `your_domain.com` with your actual domain and `3000` with your application's port.

3.  **Enable the Configuration and Restart Nginx:**


    `sudo ln -s /etc/nginx/sites-available/nextjs-app /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx`

4.  **Secure with SSL Using Let's Encrypt (Certbot):**


    `sudo apt install certbot python3-certbot-nginx -y
    sudo certbot --nginx -d your_domain.com -d www.your_domain.com`

    Follow the prompts to obtain and install SSL certificates.

### **3\. Monitoring and Logs**

-   **PM2 Monitoring Dashboard:**

    `pm2 monit`

-   **Access PM2 Logs:**


    `pm2 logs nextjs-app`

-   **Log Rotation:**

    To prevent log files from consuming too much disk space, set up log rotation with PM2:


    `pm2 install pm2-logrotate`

    Configure log rotation as needed:


    `pm2 set pm2-logrotate:max_size 10M
    pm2 set pm2-logrotate:retain 30`
