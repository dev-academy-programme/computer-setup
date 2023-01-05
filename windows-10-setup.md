# Windows 10 Computer Setup for Dev Academy

Instructions to help you set up the Linux subsystem on Windows 10.

Pro-tip: There's a copy button hidden in the top-right of each code block for easy copy/pasting.

![Screen Shot of clicking on the copy button](https://user-images.githubusercontent.com/47387/161153831-7a3ca544-0ad2-4977-aec8-92436f1a6bc5.png)

We'll be doing all our work inside a Windows Subsystem for Linux (WSL) environment. This means you'll need to work through all the setup steps below, even if you've already been using node, VS Code, etc. in your Windows environment.

There is also a video walkthrough of the setup [here](https://youtu.be/vMOvoB20flI) that you can refer to if you get stuck on any of the steps outlined below. 

<details>
<summary><b>1. Installing WSL</b></summary>
<br/>
1.1. Go to the start Menu and type `Check for updates`. Make sure that your Windows
     has the most recent updates (Optional as well as Recommended ones, but you don't
     need to upgrade to Windows 11 if you don't want to).

- Press the Windows key and type `winver` and press Enter
- Confirm that your Windows is version 2004 and higher (Build 19041 and higher) or any Windows 11 version
- If your Windows version is less than Build 19041, let one of the facilitators know

1.2. Go to the Start Menu and open Windows Powershell as an Administrator

- Put this command in Powershell `wsl --install` and hit enter
- Restart your computer

1.3. Open Ubuntu from the Start Menu and let it finish installation:

- When prompted, enter a username (e.g. your first name) and a password - this is the username that Linux will run as by default
- **IMPORTANT:** When you type in your password, you will notice nothing happens, this is a feature in Linux for security purposes.
- For Full Name, Room Number, etc. you can hit Enter to leave them blank
- If it says installing for more than 5 minutes, close the Ubuntu window and re-open it
</details>    

<details>
<summary><b>2. Configure your Windows Terminal</b></summary>
<br/>
2.1. Open your Windows Terminal application (find this by typing `Windows Terminal` in the Start menu, it is NOT the same as Powershell) 
     and select the dropdown next to the new tab button then select Settings, then "Open JSON file"

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

2.2. Next, add these properties to the Ubuntu section (the same section you got the "guid" from in the previous step)

```json
"colorScheme": "One Half Dark",
"startingDirectory": "\\\\wsl$\\Ubuntu20.04LTS\\home\\[your_Linux_username]"
```

2.3. Once you have pasted it in, your Linux section should look like this (but instead of `maia`, it would say YOUR username):

```json
        {
            "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
            "hidden": false,
            "name": "Ubuntu20.04LTS",
            "source": "Windows.Terminal.Wsl",
            "colorScheme": "One Half Dark",
            "startingDirectory": "\\\\wsl$\\Ubuntu20.04LTS\\home\\maia"
        },
```

Note that the `"source": "Windows.Terminal.Wsl",` line needs a comma at the end of it.

If you see a 'network name cannot be found' error when you load windows terminal double check that the "name" part of the json (ie "Ubuntu20.04LTS" in the             above example) exactly matches the bit between `wsl$\\[exact-name-match-here]\\home` in the "startingDirectory" part.
</details>    

<details>
<summary><b>3. Setup Visual Studio Code</b></summary>
<br/>
3.1. Install Visual Studio Code if it isn't already installed

https://code.visualstudio.com/download

3.2. In your terminal, open VS Code with:

```
code .
```

(_don't_ open VS Code from the Start Menu, desktop link or any other way)

If this is the first time you have installed VS Code you may need to close your terminal and open it again (Start menu : Windows Terminal)
to reload the VS Code settings.

3.3 Installing extensions

Install the following VS Code extensions

- ESLint
- Prettier
- Remote - WSL
- Live Share
- Handlebars (the one by André Junges)
- vscode-icons (optional, but pretty :wink:)
- GitLens (optional)

3.4 In your Ubuntu terminal, run:

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

*For some people, mysteriously, those IDs will not print out. This is okay and everything will be fine, you can move on. 
     
If you have installed these previously in Windows, you may have to reinstall them for
WSL.

3.5 Visual Studio Code settings

In VS Code:

- click the Settings cog button in the bottom left and open the Command Palette.
- type `settings.json` into the little search box that appears at the top of your scree
- click on the `Preferences: Open User Settings (JSON)` option to open your `settings.json` config file.

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
  "[javascriptreact]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.semi": false,
  "prettier.singleQuote": true
```

**NOTE:** Each entry in your `settings.json` should end in a comma, except for the last one. If there are some existing entries you'll need to add a comma to the end of this line: `"prettier.singleQuote": true`.
</details>

<details>
<summary><b>4. Installing linux software</b></summary>
<br/>
To install most of the linux software you need, we'll run three commands.

4.1 This one updates your package sources:

```sh
sudo apt-get update
```

4.2. This will install all the packages we need (it might take a while):

```sh
sudo apt-get install build-essential python-is-python3 zsh
```

4.3. Finally, this will set `zsh` as your default shell:

```sh
chsh -s $(which zsh)
```

If those all succeeded, you can restart your Ubuntu terminal, and you should be in `zsh`

If you get a page full of info about "This is the Z Shell configuration for new users...", press q (Quit and do nothing)

4.4. Run this command to confirm `python` installed correctly

```sh
which python
```

Anything except "python not found" is okay here.

4.5. Installing oh-my-zsh

We're going to install oh-my-zsh to make your terminal/shell experience a bit more pleasant.

> Oh My Zsh is a delightful, open source, community-driven framework for
> managing your Zsh configuration. It comes bundled with thousands of helpful
> functions, helpers, plugins, themes, and a few things that make you shout...

Enter this command into your Ubuntu terminal (note that it's one long line, even if it displays as two lines on the page where you're reading this):

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

4.6. Configuring ZSH

Zsh installs a command `omz` to configure itself. To set your theme to "bira" run:

```sh
omz theme set bira
```

For the remainder of these setup instructions, and at the start of Bootcamp, when we say "terminal" we mean this Ubuntu terminal, i.e. an Ubuntu tab within the Windows Terminal application -- you'll know it's right if you can see the penguin!

At the bottom of this file we'll tell you how to run a terminal within VS Code but please use the Ubuntu terminal for these setup instructions and for any `npm install` actions throughout Bootcamp, and please don't use Git Bash for any Bootcamp work

If the prompt in your terminal is now a little arrow and a tilde (~), instead of "yourname@...", that's OK (you'll change it again in a later step)

4.7. Starting in the right directory

Restart your terminal.

If your terminal is opening at a `/` (or `[user]@machineId /`) prompt instead of a `~` (or `[user]@machineId ~`) prompt, this means the terminal 
is opening at root and we need to change that.

To make it open at home (`~`) instead. We're going to run this snippet to add a couple more lines to the bottom of your `~/.zshrc` file.

```sh
cat << 'EOF' >> ~/.zshrc
if [[ $(pwd) == / ]]; then
    cd ~
fi
EOF
```

Restart your terminal.

You should now be at the home directory `~`.
</details>    
     
<details>      
<summary><b>5. Install NVM</b></summary>
<br/>
NVM is a tool to install and manage NodeJS versions.

5.1. Check if you have installed nvm before

```sh
type nvm
```

If you see something like `nvm is a shell function from /home/username/.nvm/nvm.sh` you've already installed NVM and can go to section 6. 
If you see a `nvm not found` message then keep reading.

5.2. First, check if you have node installed

```sh
which node
```

If that logs a path in "Program Files", you've installed NodeJS at some point with the official installer. 
Open Add/Remove Programs from the Start Menu and uninstall NodeJS.

If it logs "node not found", that's perfect. We want NVM to manage node and npm on our dev machine.

5.3. Enter this command into your terminal to download and install nvm:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

 This command will initialise NVM when you open a terminal

```sh
cat << 'EOF' >> ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
EOF
```

5.4. Now run this command to reload your `~/.zshrc`

```sh
omz reload
```
</details>
    
<details>    
<summary><b>6. Installing Node and NPM with NVM</b></summary>
<br/>
Install the latest "Long Term Support" (i.e. very stable) version of node

6.1. Run this command in your terminal:

```sh
nvm install --lts
```

6.2. Then, also in your terminal, run:

```sh
nvm alias default node
```

6.3. To confirm, run this command. We're expecting something in the `v18.x` range

```sh
nvm current
```
</details>
    
<details>    
<summary><b>7. Building sqlite3</b></summary>
<br/>
`sqlite3` is a database package that we use a lot during bootcamp. At this point
you should be set up with everything you need to build it.

7.1. Run this command to confirm:

```sh
npx --yes @donothing/can-u-build-sqlite3
```

If it succeeds it will log `Everything looks good`
</details>

</details>   
     
<details>     
<summary><b>8. Configure WSL</b></summary>
<br/>
8.1. Limit your WSL virtual machine so that it can't consume too much RAM

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

8.2. Restart your PC
</details>
    
<details>    
<summary><b>9. Cloning your first repo</b></summary>
<br/>
We're going to clone a repo into your ubuntu setup to make sure everything is working fine.

9.1. We'll start by creating a directory to keep all your repos in

```sh
mkdir ~/devacademy
```

9.2. and then change directory into it:

```sh
cd ~/devacademy
```

9.3. **For these next two commands, replace the name and email with your own details**

You'll need to configure git to know your name...

```sh
git config --global user.name "Firstname Lastname"
```

... and your email address. These will be recorded as the author in commits you make

```sh
git config --global user.email "your.name@example.com"
```

9.4. If you prefer git to save your credentials instead of entering them each time, you can configure git to store them

```sh
git config --global credential.helper store
```

9.5. Run this command to make visual studio code your default editor for git commit messages

```sh
git config --global core.editor "code --wait"
```

9.6. Now go to your [github tokens page](https://github.com/settings/tokens) and create a new token

- It can be called anything, but I use something like "home laptop"
- It needs the "repo" permissions so make sure to check that checkbox
- Set the expiration to 90 days, so that it lasts all bootcamp
- **make sure you copy the token before you close that tab**

9.7. From your Ubuntu terminal, clone down `clone-a-repo-test`

```sh
git clone https://github.com/dev-academy-foundations/clone-a-repo-test.git
```

9.8. Now we're going change directory into the new directory:

```sh
cd clone-a-repo-test
```

and open Visual Studio Code

```
code .
```

9.9. Now you should be looking at the clone-a-repo-test in your editor. Click on the `README.md` file to read the hidden message.

> Running `code .` from the ubuntu terminal is the way we will open Visual
> Studio Code during this course. This ensures it opens in the Ubuntu context

Run this command in your Ubuntu terminal:

```sh
Explorer.exe .
```

Windows explorer will open that directory.

This is a quick and easy way to access your Linux files from windows if you ever need to.
</details>
    
<details>    
<summary><b>10. Run checklist</b></summary>
<br/>
10.1 Run this checklist to double-check everything:

```sh
npx --yes @devacademy/checklist
```

You should see something like this (_all ticks, no crosses, 0/x failed_):

```
Shell environment:

 [ ✓ ] darwin
 [ ✓ ] $SHELL = /bin/zsh
 [ ✓ ] ZSH version = zsh 5.8 (x86_64-apple-darwin21.0)

Node setup:

 [ ✓ ] /Users/gerard/.nvm exists
 [ ✓ ] NVM config found in ~/.zshrc
 [ ✓ ] Node version = v18.12.1
 [ ✓ ] NPM version = 9.2.0

Visual studio code:

 [ ✓ ] Visual Studio Code version = 1.65.2
 [ ✓ ] Git editor is code --wait
 [ ✓ ] VSCode extension 'dbaeumer.vscode-eslint' installed
 [ ✓ ] VSCode extension 'esbenp.prettier-vscode' installed
 [ ✓ ] VSCode extension 'ms-vsliveshare.vsliveshare' installed
 [ ✓ ] VSCode extension 'eamodio.gitlens' installed
 [ ✓ ] VSCode extension 'vscode-icons-team.vscode-icons' installed

Build requirements (for node-gyp):

 [ ✓ ] Git version = git version 2.32.0 (Apple Git-132)
 [ ✓ ] Found cc = /usr/bin/cc
 [ ✓ ] Found make = /usr/bin/make
 [ ✓ ] Found python version: Python 3.8.9 at /usr/bin/python3

RESULT: (0/21) checks failed
```
</details>
