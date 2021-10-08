# MacOS and Linux Computer Setup for EDA

1. Start the terminal and make `zsh` the default shell
    - Run `zsh --version` in your terminal to determine if you already have `zsh` installed
      - If you do:
        - Run `chsh -s $(which zsh)`
        - Restart the terminal (or log out and back into your computer)
        - Move on to step 2      
      - If not, you'll need to install it by following the OS specific instructions below:
        - Linux
          - `sudo apt-get install zsh`
          - `chsh -s $(which zsh)`
          - Restart the terminal (or log out and log back in to your computer)
        - MacOS
          - Install [Homebrew](https://brew.sh/) with `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
          - Verify it is setup correctly with `brew doctor`
          - You will be requested to install the Command Line Developer Tools from Apple. Confirm by clicking Install. After the installation finished, continue installing Homebrew by hitting Return again.
          - Install zsh with `brew install zsh`
          - Make it the default shell: `sudo sh -c "echo $(which zsh) >> /etc/shells" && chsh -s $(which zsh)`
          - Restart the terminal (or log out and log back in to your computer)
1. Open a terminal and if you are prompted to make a choice, choose `q`.
1. Install oh-my-zsh from inside the terminal
    - `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
    - Restart the terminal
1. Install VS Code if it isn't already installed
1. In your terminal, open VS Code with `code .`
1. Install the following VS Code extensions
    - ESLint
    - Live Share (online students only)
    - vscode-icons (optional, but pretty :wink:)
    - Bracket Pair Colorizer (optional)
    - GitLens (optional)
1. Install nvm
    - `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
1. Move the 3 nvm lines from the bottom of `~/.bash_profile` (or if they aren't there, check out `~/.bashrc`) to the bottom of `~/.zshrc`
1. Change the oh-my-zsh theme at the top of the `.zshrc` file from `bobbyrussell` to `bira`.
1. Close and reopen your terminal 
    - If your command prompt now looks different (e.g. showing your computer name) that's a good sign
1. `nvm install --lts`
1. Make VS Code your default Git editor
    - Run this command in your terminal: `git config --global core.editor "code --wait"`
1. Enable the automatic fixing of linting errors on file save by adding this to your `settings.json`. In VS Code, click the settings cog button in the bottom left and open the Command Palette. Type in `settings.json` and click on the 'Preferences: Open Settings (JSON)' option. Paste in these contents.
    ```js
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
    ```
