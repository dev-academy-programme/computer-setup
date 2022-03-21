# Windows 10 Computer Setup for EDA

Instructions to help you set up the Linux subsystem on Windows 10

During Bootcamp we'll be doing all our work inside a Windows Subsystem for Linux (WSL) environment, whereas during Foundations you were working directly within your Windows environment. This means you'll need to work through all the setup steps below, even if you've already been using node, VS Code, etc. in your Windows environment.

## 1. Installing WSL

VIDEO WALKTHOUGH: https://www.youtube.com/watch?v=pwn4zknR5TU

Go to the start Menu and type `Check for updates`. Make sure that your Windows
has the most recent updates (Optional as well as Recommended ones, but you don't
need to upgrade to Windows 11 if you don't want to).

- Press the Windows key and type `winver` and press Enter
- Confirm that your Windows is version 2004 and higher (Build 19041 and higher) or any Windows 11 version
- If your Windows version is less than Build 19041, let one of the facilitators know

Go to the Start Menu and open Windows Powershell as an Administrator

- Put this command in Powershell `wsl --install` and hit enter
- Restart your computer

Open Ubuntu from the Start Menu and let it finish installation:

- When prompted, enter a username (e.g. your first name) and a password - this is the username that Linux will run as by default
- **IMPORTANT:** When you type in your password, you will notice nothing happens, this is a feature in Linux for security purposes.
- For Full Name, Room Number, etc. you can hit Enter to leave them blank
- If it says installing for more than 5 minutes, close the Ubuntu window and re-open it

## 2. Configure your Windows Terminal

Open your Windows Terminal application (find this by typing `Windows Terminal` in the Start menu, it is NOT the same as Powershell) and select the dropdown next to the new tab button then select Settings, then "Open JSON file"

Scroll down until you find this section:

```json
{
    "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
    "hidden": false,
    "name": "Ubuntu-20.04",
    "source": "Windows.Terminal.Wsl"
},
```

Copy the value of the "guid" field, then paste it in to the `defaultProfile` property near the top of the file.

```json
"defaultProfile": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
```

Next, add these properties to the Ubuntu section (the same section you got the "guid" from in the previous step)

```json
"colorScheme": "One Half Dark",
"startingDirectory": "//wsl$/Ubuntu/home/[your_Linux_username]"
```

Once you have pasted it in, your Linux section should look like this (but instead of `maia`, it would say YOUR username):

```json
{
    "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
    "hidden": false,
    "name": "Ubuntu-20.04",
    "source": "Windows.Terminal.Wsl",
    "colorScheme": "One Half Dark",
    "startingDirectory": "//wsl$/Ubuntu/home/maia"
},
```

## 3. Setup Visual Studio Code

Install Visual Studio Code if it isn't already installed

https://code.visualstudio.com/download

In your terminal, open VS Code with:

```
code .
```

(_don't_ open VS Code from the Start Menu, desktop link or any other way)

### 3.1 Installing extensions

Install the following VS Code extensions

- ESLint
- Prettier
- Remote - WSL
- Live Share
- vscode-icons (optional, but pretty :wink:)
- GitLens (optional)

In your Ubuntu terminal, run:

```shell
code --list-extensions
```

You should see the IDs of each of these extensions logged like this:

```
dbaeumer.vscode-eslint
esbenp.prettier-vscode
ms-vsliveshare.vsliveshare
vscode-icons-team.vscode-icons
eamodio.gitlens
```

If you have installed these previously in Windows, you may have to reinstall them for
WSL.

### 3.2 Visual Studio Code settings

In VS Code:

1. click the Settings cog button in the bottom left and open the Command Palette.
2. Type `settings.json` into the little search box that appears at the top of your scree
3. click on the `Preferences: Open Settings (JSON)` option to open your `settings.json` config file.

Paste these contents inside the curly brackets:

```json
 "editor.detectIndentation": false,
 "editor.insertSpaces": true,
 "editor.tabSize": 2,
 "editor.codeActionsOnSave": { "source.fixAll.eslint": true },
 "editor.bracketPairColorization.enabled": true,
 "editor.guides.bracketPairs":"active",
 "[javascript]": {
   "editor.formatOnSave": true,
   "editor.defaultFormatter": "esbenp.prettier-vscode"
 },
 "prettier.semi": false,
 "prettier.singleQuote": true
```

Note that each entry in your `settings.json` should end in a comma except for the last one, so if there are some existing entries you'll need to add a comma before pasting the above lines

### 3.3 Make VS Code your default Git editor

Run this command in your terminal:

```sh
git config --global core.editor "code --wait"
```

## 4. Installing linux software

To install most of the linux software you need, we'll run three commands.

This one updates your package sources:

```sh
sudo apt-get update
```

This will install all the packages we need (it might take a while):

```sh
sudo apt-get install build-essential python-is-python3 zsh
```

finally, this will set `zsh` as your default shell:

```sh
chsh -s $(which zsh)
```

If those all succeeded, you can restart your Ubuntu terminal, and you should be in `zsh`.

If you get a page full of info about "This is the Z Shell configuration for new users...", press q (Quit and do nothing)

### 4.1 Installing oh-my-zsh

oh-my-zsh is a package that

Enter this command into your Ubuntu terminal (note that it's one long line, even if it displays as two lines on the page where you're reading this):

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 4.2 Configuring ZSH

Zsh uses a file in your home directory `~/.zshrc` to set your theme to "bira" run:

```sh
omz theme set bira
```

For the remainder of these setup instructions, and at the start of Bootcamp, when we say "terminal" we mean this Ubuntu terminal, i.e. an Ubuntu tab within the Windows Terminal application -- you'll know it's right if you can see the penguin!

At the bottom of this file we'll tell you how to run a terminal within VS Code but please use the Ubuntu terminal for these setup instructions and for any `npm install` actions throughout Bootcamp, and please don't use Git Bash for any Bootcamp work

If the prompt in your terminal is now a little arrow and a tilde (~), instead of "yourname@...", that's OK (you'll change it again in a later step)

### 4.3 Starting in the right directory

Restart your terminal.

If your terminal is opening at a `/` (or `[user]@machineId /`) prompt instead of a `~` (or `[user]@machineId ~`) prompt, this means the terminal is opening at root.

To make it open at home (`~`) instead. We're going to run this snippet to add a couple more lines to the bottom of your `~/.zshrc` file.

```sh
cat << EOF >> ~/.zshrc
if [[ $(pwd) == / ]]; then
    cd ~
fi
EOF
```

Restart your terminal.

You should now be at the home directory `~`.

### 4.4 Install NVM

NVM is a tool to install and manage NodeJS versions.

First, check if you have node installed

```sh
which node
```

If that logs a path in "Program Files", you've installed NodeJS at some point with the official installer. Open Add/Remove Programs from the Start Menu and uninstall NodeJS.

If it logs "node not found", that's perfect. We want NVM to manage node and npm on our dev machine.

Enter this command into your terminal to download and install nvm:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
```

This command will initialise NVM when you open a terminal

```sh
cat << EOF >> ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
EOF
```

### 4.5 Installing Node and NPM with NVM

Install the latest "Long Term Support" (i.e. very stable) version of node

Run this command in your terminal:

```sh
nvm install --lts
```

Then, also in your terminal, run:

```sh
nvm alias default node
```

To confirm, run this command. We're expecting something in the `v16.x` range

```
nvm current
```

## 5. Configure WSL

Limit your WSL virtual machine so that it can't consume too much RAM

- In File Explorer (aka "Windows Explorer" or "This PC"), go to `C:\Users\[your_Windows_username]`
- On the `View` tab, tick the checkboxes for `File name extensions` and `Hidden items`
- Right-click in your `C:\Users\[your_Windows_username]` folder and choose `New` then `Text Document`; this will open up Notepad
- Rename the new file as `.wslconfig`, making sure that it does NOT have an extra `.txt` extension
- Paste this text into body of the `.wslconfig` file, and then save it:

```
[wsl2]
memory=2GB # Limits VM memory in WSL2 to 2 GB
processors=2 # Makes the WSL2 VM use two virtual processors
```

Restart your PC

## 6. You're all set up

```sh
npx @donothing/checklist
```

```
cat << EOF >> /etc/wsl.conf
[user]
default=michael
EOF

```
