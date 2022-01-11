# MacOS and Linux Computer Setup for EDA

If you haven't already done so, please DO NOT upgrade your iOS (Mac version) to the latest version, Monterey. This is incompatible with Discord screen share and we're waiting on Discord to provide a fix.

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
    - GitLens (optional)
1. Set up your VS Code editor to treat a tab as 2 spaces (coding in pairs and teams is easier if we all follow the same style convention!): 
    - In VS Code, click on the `Settings` icon (the cog in the bottom left corner)
    - In the `Search settings` box at the top, type `tab`
    - Check/update the following settings: 
        - `Detect Indentation` should be unticked
        - `Insert Spaces` should be ticked
        - `Tab Size` should be 2
1. Install nvm
    - Type `nvm` in your terminal to determine whether you already have nvm, if it returns a bunch of text about Node Version Manager you can skip this step 
    - `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
1. Move the 3 nvm lines from the bottom of `~/.bash_profile` (or if they aren't there, check out `~/.bashrc`) to the bottom of `~/.zshrc`
1. Change the oh-my-zsh theme at the top of the `.zshrc` file from `bobbyrussell` to `bira`
1. Close and reopen your terminal 
    - If your command prompt now looks different (e.g. showing your computer name) that's a good sign
1. Install node
    - Type `node -v` in your terminal to determine whether you already have node, if it returns a version number, you can skip this step
    - `nvm install --lts`
3. Make VS Code your default Git editor
    - Run this command in your terminal: `git config --global core.editor "code --wait"`
4. Enable automatic colour-coding of brackets and automatic fixing of linting errors on file save
    - In VS Code, click the Settings cog button in the bottom left and open the Command Palette. Type `settings.json` into the little search box that appears at the top of your screen, and then click on the `Preferences: Open Settings (JSON)` option to open your `settings.json` config file. Paste in these contents:
    ```json
    "editor.bracketPairColorization.enabled": true,
    "editor.codeActionsOnSave": { "source.fixAll.eslint": true }
    ```
    - Note that each entry in your `settings.json` should end in a comma except for the last one, so if there are some existing entries you'll need to add a comma before pasting the above lines
