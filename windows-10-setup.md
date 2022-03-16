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

## 3. Installing linux software

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

### 3.1 Installing oh-my-zsh

oh-my-zsh is a package that

Enter this command into your Ubuntu terminal (note that it's one long line, even if it displays as two lines on the page where you're reading this):

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 3.2 Configuring ZSH

Zsh uses a file in your home directory `~/.zshrc` to set your theme to "bira" run:

```sh
omz theme set bira
```

For the remainder of these setup instructions, and at the start of Bootcamp, when we say "terminal" we mean this Ubuntu terminal, i.e. an Ubuntu tab within the Windows Terminal application -- you'll know it's right if you can see the penguin!

At the bottom of this file we'll tell you how to run a terminal within VS Code but please use the Ubuntu terminal for these setup instructions and for any `npm install` actions throughout Bootcamp, and please don't use Git Bash for any Bootcamp work

If the prompt in your terminal is now a little arrow and a tilde (~), instead of "yourname@...", that's OK (you'll change it again in a later step)

### 3.3 Starting in the right directory

Restart your terminal.

If your terminal is opening at a `/` (or `[user]@machineId /`) prompt instead of a `~` (or `[user]@machineId ~`) prompt, this means the terminal is opening at root.

To make it open at home (`~`) instead. We're going to run this snippet to add a couple more lines to the bottom of your `~/.zshrc` file.

```sh
cat << EOF >> ~/.zshrc
if [[$(pwd) == /]]; then
    cd ~
fi
EOF
```

Restart your terminal.

You should now be at the home directory `~`.

### 3.4 Install NVM

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

### 3.5 Installing Node and NPM with NVM

Install the latest "Long Term Support" (i.e. very stable) version of node

Run this command in your terminal:

```sh
nvm install --lts
```

Then, also in your terminal, run:

```sh
nvm alias default node
```

## 4. Setup Visual Studio Code

Install Visual Studio Code if it isn't already installed

https://code.visualstudio.com/download

In your terminal, open VS Code with:

```
code .
```

(_don't_ open VS Code from the Start Menu, desktop link or any other way)

### 4.1 Installing extensions

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

### 4.2 Visual Studio Code settings

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

### 4.3 Make VS Code your default Git editor

Run this command in your terminal:

```sh
git config --global core.editor "code --wait"
```

## 6. Configure WSL

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

# Testing your setup

Test that you have Node.js installed

- Open a new Ubuntu terminal (NOT Git Bash!) and type `node`
- You should see something like this (the exact version number doesn't matter but it should start with `16`):

```

$ node
Welcome to Node.js v16.13.1.
Type ".help" for more information.

>

```

- This is called the REPL ("Read Evaluate Print Loop")
- You should be able to write simple Javascript here, e.g. try typing `2 + 2` or `Math.random()`
- When you've finished trying it out, do `Ctrl-C` then `Ctrl-C` again, to get out of the REPL

1. Test that you can you can clone a repo

- From your Ubuntu terminal, clone down any Foundations repo, e.g. `git clone https://github.com/dev-academy-foundations/javascript-carnival` (you've probably already cloned this into your Windows filesystem using Git Bash, but now you're cloning it a second time, into your Linux filesystem)
  - If you use HTTPS when connecting to GitHub, you'll need to put in your GitHub _token_ when it asks for a password, not your regular GitHub password
    - After you've copied the token to your clipboard, right-click once at the Password prompt to paste it-- you won't see anything pasted, but it will have been pasted and then you can click Enter
  - Using SSH when connecting to GitHub is also fine
  - For the repo you just cloned, cd ("change directory") into that folder, e.g. `cd javascript carnival`, then do `code .`
  - This should open VS Code and you should be able to see the code from that repo

1. If any of these steps didn't work as expected, please ask for help in Slack or Discord (ideally during week 5 of Foundations, but at least during the week _before_ you start Bootcamp)

## Where to save your files

We recommend that you store all your Bootcamp files, folders and repos within your WSL (Linux) filesystem. This should happen automatically when you create directories, clone repos, etc. from the Linux command prompt. We recommend you DON'T store your Bootcamp code in a Windows directory like 'My Documents'.

If you need to access your Linux folder structure from Windows Explorer, the path will be something like this:

**WSL 1:** `C:\Users\[your_Windows_username]\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\rootfs\home\[your_Linux_username]`

- The path might be slightly different on your computer - if you can't figure it out, then in Windows Explorer do a search for `rootfs` with 'Kind' set to 'folder' (this search might take ten minutes or so).

**WSL 2:** `\\wsl$\Ubuntu-20.04\home\[your_Linux_username]` or `\wsl.localhost\Ubuntu-20.04\home\[your_Linux_username]`

Optional: You won't need this very often but you might like to make a Windows shortcut to this folder just in case.

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

```

```
