# Windows 10 Computer Setup for EDA

1. Install the Windows Subsystem for Linux
    - Control Panel -> Programs -> Turn Windows Features on or off
    - Select Windows Subsystem for Linux -> OK
    - Restart computer
1. Install Ubuntu 20 and the Windows Terminal from the Windows Store
    - If you can't install from the Windows Store, make sure you're signing in with your Microsoft Account and have verified your device (Settings -> Accounts)
    - Also, make sure you're using the most recent version of Windows 10. On the Windows Terminal page in the Store, select System Requirements -> Update
    - Open Ubuntu and let it finish installation. If it says installing for more than 5 minutes, close the Ubuntu window and re-open it
1. Configure your Windows Terminal
    - Select the dropdown next to the new tab button and select Settings
    - Change the value of the `defaultProfile` property to the value of the Ubuntu's `guid` property
    - Add these properties to the Ubuntu section
    ```js
    "colorScheme": "One Half Dark",
    "startingDirectory": "//wsl$/Ubuntu-20.04/home/[your_username]"
    ```
1. Restart the Windows Terminal and install zsh and make it the default shell
    - `sudo apt-get install zsh`
    - `chsh -s $(which zsh)`
    - Restart the Windows Terminal
1. Install oh-my-zsh from inside the Windows Terminal
    - `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
    - Restart the terminal and open an Ubuntu tab
1. Install VS Code if it isn't already installed
1. In your Ubuntu terminal, open VS Code with `code .`
1. Install the following VS Code extensions
    - ESLint
    - WSL - Remote
    - Live Share (online students only)
    - vs-code-icons (optional)
    - GitLens (optional)
1. Restart your terminal
1. Open VS Code again with `code .`
    - This should begin downloading the VS Code Server
    - When prompted for access, accept it
1. Install nvm
    - `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
1. Move the 3 nvm lines from the bottom of `.bashrc` to the bottom of `.zshrc`
1. Change the oh-my-zsh theme to 'bira'
1. `nvm install --lts`
1. In your Windows Terminal install some global npm packages
    ```sh
    npm install -g \
    eslint@5.16.0 \
    babel-eslint@10.0.1 \
    eslint-config-standard@12.0.0 \
    eslint-plugin-import@2.17.3 \
    eslint-plugin-node@9.1.0 \
    eslint-plugin-promise@4.1.1 \
    eslint-plugin-react@7.13.0 \
    eslint-plugin-standard@4.0.0
    ```
1. Add a global ESLint config file to your home folder
    - Run `code .` in your Ubuntu terminal
    - Paste in these contents
    ```js
    {
      "parser": "babel-eslint",
      "env": {
        "browser": true,
        "node": true,
        "jest": true
      },
      "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "standard"
      ],
      "plugins": [
        "standard",
        "promise"
      ],
      "rules": {
        "eol-last": ["error", "always"],
        "no-multiple-empty-lines": [
          "error", { "max": 1, "maxEOF": 0, "maxBOF": 0 }
        ],
        "object-curly-spacing": [2, "always"],
        "react/prop-types": "off"
      },
      "settings": {
        "react": { "version": "detect" }
      }
    }
    ```
    - Save the file as `.eslintrc.json` in your home folder.
1. Add a shortcut key to auto-format JavaScript
    ```js
    [
      {
        "key": "shift+alt+b",
        "command": "eslint.executeAutofix",
        "when": "editorFocus"
      }
    ]
    ```
