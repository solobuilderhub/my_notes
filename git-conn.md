### Signing into github from unix:

**Part 1: Cloning Your GitHub Repository**
------------------------------------------

### **Option 1: Using SSH Authentication**

SSH is a secure and convenient method to authenticate with GitHub without needing to enter your username and password each time. Here's how to set it up:

#### **1\. Generate an SSH Key (If You Don't Have One)**

First, check if you already have an SSH key:

bash

Copy code

`ls -al ~/.ssh`

Look for files named `id_rsa` and `id_rsa.pub` or `id_ed25519` and `id_ed25519.pub`. If you don't have them, generate a new SSH key:

bash

Copy code

`ssh-keygen -t ed25519 -C "your_email@example.com"`

*If your system doesn't support `ed25519`, use `rsa`:*

bash

Copy code

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

**Follow the prompts:**

-   **File location:** Press `Enter` to accept the default location.
-   **Passphrase:** (Optional) Enter a secure passphrase or press `Enter` to leave it empty.

#### **2\. Add Your SSH Key to the SSH-Agent**

Start the SSH agent in the background:

bash

Copy code

`eval "$(ssh-agent -s)"`

Add your SSH private key to the agent:

bash

Copy code

`ssh-add ~/.ssh/id_ed25519`

*Replace `id_ed25519` with your key filename if different.*

#### **3\. Add Your SSH Key to Your GitHub Account**

1.  **Copy the SSH Public Key:**

    bash

    Copy code

    `cat ~/.ssh/id_ed25519.pub`

    *Copy the entire output, which starts with `ssh-ed25519` or `ssh-rsa`.*

2.  **Add the Key to GitHub:**

    -   Log in to your GitHub account.
    -   Navigate to **Settings** > **SSH and GPG keys**.
    -   Click **New SSH key**.
    -   **Title:** Provide a descriptive name (e.g., `Azure VM SSH Key`).
    -   **Key:** Paste the copied public key.
    -   Click **Add SSH key**.

#### **4\. Test the SSH Connection**

Ensure that your SSH setup works:

bash

Copy code

`ssh -T git@github.com`

You should see a message like:

vbnet

Copy code

`Hi solobuilderhub! You've successfully authenticated, but GitHub does not provide shell access.`

#### **5\. Clone the Repository Using SSH**

Now, clone your repository using the SSH URL:

bash

Copy code

`git clone git@github.com:solobuilderhub/linkwise.git`

*Ensure that you're using the correct SSH URL format.*

* * * * *

### **Option 2: Using HTTPS with a Personal Access Token (PAT)**

If you prefer using HTTPS, you can authenticate using a PAT instead of a password.

#### **1\. Generate a Personal Access Token (PAT)**

1.  **Navigate to GitHub Settings:**

    -   Log in to GitHub.
    -   Go to **Settings** > **Developer settings** > **Personal access tokens**.
2.  **Generate a New Token:**

    -   Click **Generate new token** > **Generate new token (classic)**.
    -   **Note:** GitHub is transitioning to fine-grained tokens; ensure you're using the latest method available.
3.  **Configure the PAT:**

    -   **Note:** Select the necessary scopes. For cloning repositories, the **`repo`** scope is sufficient.
    -   **Expiration:** Set an appropriate expiration date.
    -   Click **Generate token**.
4.  **Copy the PAT:**

    -   **Important:** Copy the generated token immediately as you won't be able to view it again.

#### **2\. Clone the Repository Using HTTPS and PAT**

Use the PAT as your password when cloning:

bash

Copy code

`git clone https://github.com/solobuilderhub/linkwise.git`

When prompted:

-   **Username:** Enter your GitHub username (`solobuilderhub`).
-   **Password:** Enter the PAT you generated.

*For security, it's recommended to use SSH over HTTPS with PAT.*

#### **3\. Cache Your Credentials (Optional)**

To avoid entering your PAT every time, you can cache your credentials:

bash

Copy code

`git config --global credential.helper cache`

*Or, to store them permanently (not recommended for security reasons):*

bash

Copy code

`git config --global credential.helper store`
