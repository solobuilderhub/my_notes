In a Linux shell environment, you can use several command-line text editors to edit files. Here are a few common and easy-to-use editors:

### 1\. **Nano** (Simple and User-Friendly)

`nano` is a simple and user-friendly text editor for Linux. If you just want to quickly remove all content and paste new code, `nano` is a great choice.

#### How to Use `nano`:

-   Open the file with `nano`:

    bash

    Copy code

    `sudo nano /etc/nginx/sites-available/default`

-   To remove all content:

    -   Press `Ctrl + K` repeatedly to cut all lines of the existing code.
-   To paste new content:

    -   Copy your new code (e.g., from your local machine or elsewhere).
    -   In `nano`, right-click inside the terminal to paste (if using a terminal emulator), or press `Shift + Ctrl + V`.
-   Save the file:

    -   Press `Ctrl + O` to write out (save) the file.
    -   Press `Enter` to confirm the filename.
-   Exit `nano`:

    -   Press `Ctrl + X` to exit.

* * * * *

### 2\. **Vim** (Advanced, but Powerful)

`vim` is a more advanced text editor, but if you're comfortable with it, it can be a powerful tool for editing files.

#### How to Use `vim`:

-   Open the file with `vim`:

    bash

    Copy code

    `sudo vim /etc/nginx/sites-available/default`

-   To remove all content:

    -   Press `gg` to move to the top of the file.
    -   Press `dG` to delete all lines from the current position to the end.
-   To enter paste mode:

    -   Press `i` to enter **Insert** mode.
    -   Right-click to paste the new code or use `Shift + Ctrl + V`.
-   Save the file:

    -   Press `Esc` to exit **Insert** mode.
    -   Type `:wq` and press `Enter` to write (save) and quit.
