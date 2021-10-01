# Windows 10 Computer Setup for EDA
Instructions to help you set up the Linux subsystem on Windows 10

VIDEO WALKTHOUGH: https://www.youtube.com/watch?v=pwn4zknR5TU 

1. Install the Windows Subsystem for Linux
    - Control Panel -> Programs -> Turn Windows Features on or off
    - Select Windows Subsystem for Linux -> OK
    - Restart computer
1. Install Ubuntu 20 and the Windows Terminal from the Windows Store
    - If you can't install from the Windows Store, make sure you're signing in with your Microsoft Account and have verified your device (Settings -> Accounts)
    - Also, make sure you're using the most recent version of Windows 10. On the Windows Terminal page in the Store, select System Requirements -> Update
    - Open Ubuntu and let it finish installation
        - When prompted, enter a username (e.g. your first name) and a password - this is the username that Linux will run as by default
        - For Full Name, Room Number, etc. you can hit Enter to leave them blank
        - If it says installing for more than 5 minutes, close the Ubuntu window and re-open it
1. Configure your Windows Terminal
    - Open your Windows Terminal application and select the dropdown next to the new tab button then select Settings
    - Scroll down until you find this section:
 
    ```js
       {
                "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
                "hidden": false,
                "name": "Ubuntu-20.04",
                "source": "Windows.Terminal.Wsl"
       },
    ```

    - Copy the value of the "guid" field, then paste it in to the `defaultProfile` property near the top of the file.
    
    ```js
        "defaultProfile": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
    ```
    
    - Next, add these properties to the Ubuntu section (the same section you got the "guid" from in the previous step) 
    ```js
    "colorScheme": "One Half Dark",
    "startingDirectory": "//wsl$/Ubuntu-20.04/home/[your_Linux_username]"
    ```
    - Once you have pasted it in, your Linux section should look like this (but instead of `maia`, it would say YOUR username):

    ```js
            {
                "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
                "hidden": false,
                "name": "Ubuntu-20.04",
                "source": "Windows.Terminal.Wsl",
                "colorScheme": "One Half Dark",
                "startingDirectory": "//wsl$/Ubuntu-20.04/home/maia"
            },
    ```
1. Install some required libraries including zsh, make zsh the default shell, and restart your Windows Terminal
    - Put this command into your terminal: `sudo apt-get install build-essential python-is-python3 zsh`
    - Then this command: `chsh -s $(which zsh)`
    - Restart the Windows Terminal
    - If you get a page full of info about "This is the Z Shell configuration for new users...", press q (Quit and do nothing)
1. Install oh-my-zsh from inside the Windows Terminal
    - Enter this command into your terminal (note that it's one long line, even if it displays as two lines on the page where you're reading this): `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
    - Restart the terminal and open an Ubuntu tab with the little plus button in the top left corner
    - If the prompt in your terminal window is now a little arrow and a tilde (~), instead of "yourname@...", that's OK (you'll change it again in a later step)
1. Install VS Code if it isn't already installed
    - https://code.visualstudio.com/download
1. In your Ubuntu terminal, open VS Code with `code .`
1. Install the following VS Code extensions
    - ESLint
    - Remote - WSL
    - Live Share (online students only)
    - vscode-icons (optional, but pretty :wink:)
    - Bracket Pair Colorizer (optional)
    - GitLens (optional)
1. Restart your terminal
1. Open VS Code again with `code .`
    - This should begin downloading the VS Code Server
    - When prompted for access (by Windows Defender Firewall), click "Allow access"
1. Install nvm
    - Enter this command into your terminal: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
1. Move the 3 nvm lines from the bottom of `.bashrc` to the bottom of `.zshrc`
    - This is the trickiest step! 
    - Run this command to open your .bashrc config file: `code ~/.bashrc`
    - Scroll down to the bottom of the file and cut the three lines at the bottom that look like this: 

    ```
        export NVM_DIR="$HOME/.nvm"
        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
        [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    ```

    - Run this command to open your .zshrc file: `code ~/.zshrc`
    - Paste the nvm lines you cut from the .bashrc file down at the bottom of the .zshrc file
1. Get rid of the distracting green highlighting in your terminal (optional, but recommended):
    - In your .zshrc file (that you opened in the above step), find this line and uncomment it (i.e. delete the `#`):
    ```
    # DISABLE LS_COLORS="true"
    ```
1. Change the oh-my-zsh theme to 'bira'
    - Scroll to the top of the .zshrc file and replace the `ZSH_THEME=` value with 'bira'
1. Restart your terminal
1. Install the latest version of nvm 
    - Run this command in your terminal: `nvm install --lts`
1. Make VS Code your default Git editor
    - Run this command in your terminal: `git config --global core.editor "code --wait"`
1. Enable the automatic fixing of linting errors on file save by adding this to your `settings.json` in VS Code:
    ```js
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
    ```
1. Restart your PC

## Where to save your files
We recommend that you store all your Bootcamp files, folders and repos within your WSL (Linux) filesystem. This should happen automatically when you create directories, clone repos, etc. from the Linux command prompt. We recommend you DON'T store your Bootcamp code in a Windows directory like 'My Documents'.

If you need to access your Linux folder structure from Windows Explorer, the path will be something like this: `C:\Users\[your_Windows_username]\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\rootfs\home\[your_Linux_username]`
- Optional: You won't need this very often but you might like to make a Windows shortcut to this folder just in case. The path might be slightly different on your computer - if you can't figure it out, then in Windows Explorer do a search for `rootfs` with 'Kind' set to 'folder' (this search might take ten minutes or so).

If you need to navigate to your Windows folder structure from Linux, the path will be something like this: `/mnt/c/Users/[your_Windows_username]/Documents`

### Optional - how to copy files from the Windows filesystem to the Linux filesystem
If you already have some files in a Windows folder (e.g. your Foundations repos) and you want to copy them into the Linux folder structure: 
1. Make sure the Windows folder that your files are in has no spaces in the filename (if there are spaces, then rename the folder without spaces)
1. Suppose your files are in `C:/Users/[your_Windows_username]/Documents/Dev-Academy` and you want to copy them to a new `dev-academy` folder in Linux... then, in your terminal paste this command: `cp -r /mnt/c/Users/[your_Windows_username]/Documents/Dev-Academy dev-academy`
    - Make sure you replace `c/Users/[your_Windows_username]/Documents/Dev-Academy` with your actual filepath and username
    - If you have lots of files this may take several hours to run
    - This will create the new `dev-academy` folder, you don't need to do `mkdir`
    - Don't try to use Windows Explorer to copy the files to Linux, because they will end up with the wrong permissions in Linux
    - Once you're sure your files are safely in Linux, you can delete them from the Windows file structure

## Running VS Code in WSL/Ubuntu mode 

- You'll need this information once you start Bootcamp, but you don't need to do anything now

During Bootcamp you should open VS Code by typing `code .` from your Ubuntu terminal, not by opening it from your Start menu, desktop etc. You'll know you've done it right because your VS Code window will have a green rectangle with ">< WSL:Ubuntu-20.04" in the bottom left-hand corner. If your green rectange is smaller and only has "><" then you're running VS Code in Windows/local mode, which is not what you want.
- If your "green rectangle" is actually mauve or some other colour, that's OK, so long as it says ">< WSL:Ubuntu-20.04"!

You can open a terminal within VS Code by choosing Terminal -> New Terminal from the VS Code menu, or Ctrl + Shift + \` (that character is called a backtick - it's at the top left of your keyboard, on the same key as the "~" tilde character). It's fine to use this VS Code terminal for most purposes (running your code, git commits, etc.). But when you do `npm install` actions you should do them in your actual Ubuntu terminal, not in the terminal within VS Code. If you don't know what `npm install` means yet, don't worry, you will soon!

Once you have one VS Code window open, you can open another one (e.g. for Liveshare) by pressing F1, then selecting "Remote-WSL: New WSL Window". 

