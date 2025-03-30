# MacOS Computer Setup for EDA

If you are using the latest MacOS (Monterey) please note that you need to be running the latest version of the Discord desktop app for screen sharing to function. If you encounter problems, you can use Discord in a Safari browser.

---

Pro-tip: There's a copy button hidden in the top-right of each code block for easy copy/pasting.

![Screen Shot of clicking on the copy button](https://user-images.githubusercontent.com/47387/161153831-7a3ca544-0ad2-4977-aec8-92436f1a6bc5.png)

This guide is mostly running commands in the Terminal app.

### 1. Installing XCode Command-line Developer Tools

Running this command will show a system prompt, asking you to confirm in order to install the command-line developer tools:

```sh
xcode-select --install
```

## 2. Configuring ZSH

MacOS ships with ZSH, to double check that you have it installed run this command in your terminal

```sh
which zsh
```

It should log `/bin/zsh` but anything except `zsh not found` is great

Run this command to set `zsh` as your default shell:

```sh
chsh -s $(which zsh)
```

Close and open a new terminal and if you are prompted to make a choice, choose `q`.

### 2.1 Installing oh-my-zsh

We're going to install oh-my-zsh to make your terminal/shell experience a bit more pleasant.

> Oh My Zsh is a delightful, open source, community-driven framework for
> managing your Zsh configuration. It comes bundled with thousands of helpful
> functions, helpers, plugins, themes, and a few things that make you shout...

Copy this command into your terminal

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 2.2 Configuring ZSH

Zsh installs a command `omz` to configure itself. To set your theme to "bira" run:

```sh
omz theme set bira
```

Close your terminal and open a new one.

## 3. Preparing build tools

## 3.2 Setting up python

We need python installed and Mac OS ships with `python3`

Confirm that python is installed with:

```sh
which python3
```

This should log something like `/bin/python3`, but anything other than `python3 not found` is great.

Running this command will configure NPM to use your `python3` for builds

```sh
cat << EOF >> ~/.npmrc
python=$(which python3)
EOF
```

## 4. Setup Visual Studio Code

Install Visual Studio Code if it isn't already installed

https://code.visualstudio.com/download

In your terminal, open VS Code with:

```
code .
```

If you see a 'command not found: code' error, try opening VS code by clicking on the Visual Studio Code program in your Applications folder and then open the command pallette (command + shift + p) and run `Shell Command: install 'code' command in PATH`.

### 4.1 Installing extensions

Install the following VS Code extensions

- ESLint
- Prettier
- Live Share
- vscode-icons (optional, but pretty :wink:)
- GitLens (optional)

In your terminal, run:

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

### 4.2 Visual Studio Code settings

In VS Code:

1. Click the Settings cog button in the bottom left and open the Command Palette.
2. Type `settings.json` into the little search box that appears at the top of your screen.
3. Click on the `Preferences: Open User Settings (JSON)` option to open your `settings.json` config file.

4. Paste these contents inside the curly brackets:

```json
  "editor.detectIndentation": false,
  "editor.insertSpaces": true,
  "editor.tabSize": 2,
  "editor.codeActionsOnSave": { "source.fixAll.eslint": "explicit" },
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
  "[html]": {
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.semi": false,
  "prettier.singleQuote": true
```

**NOTE:** Each entry in your `settings.json` should end in a comma, except for the last one. If there are some existing entries you'll need to add a comma to the end of this line: `"prettier.singleQuote": true`.

5. Save your `settings.json` file.

Note that each entry in your `settings.json` should end in a comma except for the last one, so if there are some existing entries you'll need to add a comma before pasting the above lines.

See [Accessibility of code in VS Code or the terminal](code-accessibility.md) for suggestions on how you might customise your setup for readability.

### 4.3 Make VS Code your default Git editor

Run this command in your terminal:

```sh
git config --global core.editor "code --wait"
```

## 5. Preparing to use Node

### 5.1 Install NVM

NVM is a tool to install and manage NodeJS versions.

First, check if you have installed nvm before

```sh
type nvm
```

If you see something like `nvm is a shell function from /home/username/.nvm/nvm.sh` you've already installed NVM and can go to section 4.5, if you see a `nvm not found` message then keep reading.

Next, check if you have node installed

```sh
which node
```

If it logs "node not found", that's perfect. We want NVM to manage node and npm on our dev machine.

If it logs anything else e.g. `/usr/bin/node`, you need to uninstall node, this differs depending on
how you installed it. Contact a facilitator if you get stuck

Enter this command into your terminal to download and install nvm:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

This command will initialise NVM when you open a terminal

```sh
cat << 'EOF' >> ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
EOF
```

Now run this command to reload your `~/.zshrc`

```sh
omz reload
```

### 4.2 Installing Node and NPM with NVM

Install the latest "Long Term Support" (i.e. very stable) version of node

Run this command in your terminal:

```sh
nvm install --lts
```

Then, also in your terminal, run:

```sh
nvm alias default node
```

To confirm, run this command. We're expecting something in the `v18.x` range

```sh
nvm current
```

### 5.3 Building sqlite3

`sqlite3` is a database package that we use a lot during bootcamp. At this point
you should be set up with everything you need to build it.

Run this command to confirm:

```sh
npx --yes @donothing/can-u-build-sqlite3
```

If it succeeds it will log `Everything looks good`

## 6. Cloning your first repo

We're going to clone a repo to make sure everything is working fine.

We'll start by creating a directory to keep all your repos in (it doesn't really matter what you call it)

```sh
mkdir ~/devacademy
```

and then change directory into it:

```sh
cd ~/devacademy
```

## 6.1 Generate an SSH key pair

There's a chance you have one of these. You can see a list of your public keys like this:

```sh
ls ~/.ssh/*.pub
```

If you can see one, skip to 6.2 Adding your ssh key to Github

If you don't see any, then you can create one. Don't forget to replace the email address with your real one.

```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Hit enter 3 times to accept all the defaults.

Now you need to start your ssh-agent:

```sh
eval "$(ssh-agent -s)"
```

and add the key to your agent:

```sh
ssh-add ~/.ssh/id_ed25519
```

## 6.2 Adding your ssh key to Github

Open the file in VS Code:

```sh
code ~/.ssh/id_ed25519.pub
```

Select-all and copy the key.

Now you'll want to go to [[https://github.com/settings/keys]], click on "New SSH Key" and paste your new key into the textfield.

**For these next two commands, replace the name and email with your own details**

You'll need to configure git to know your name...

```sh
git config --global user.name "Firstname Lastname"
```

... and your email address. These will be recorded as the author in commits you make

```sh
git config --global user.email "your.name@example.com"
```

run this command to configure git to always use ssh urls from github.com.

```sh
git config --global url.'git@github.com:'.insteadof https://github.com/
```

run this command to configure git to always use 'main' as the default branch (instead of 'master')

```sh
git config --global init.defaultBranch main
```

Since Mac OS generates `.DS_Store` files and we never want to commit them, we'll create a global excludes file for our user (this is three separate commands):

```sh
mkdir -p ~/.config/git
echo '.DS_Store' >> ~/.config/git/ignore
git config --global core.excludesFile "${HOME}/.config/git/ignore"
```

From your terminal, clone down `clone-a-repo-test`

```sh
git clone git@github.com:dev-academy-foundations/clone-a-repo-test.git
```

Now we're going change directory into the new directory:

```sh
cd clone-a-repo-test
```

and open Visual Studio Code

```
code .
```

Now you should be looking at the clone-a-repo-test in your editor. Click on the README.md file to read the hidden message.

Run this command in your terminal:

```sh
open .
```

Finder will open that directory

## 7. You're all set up

Run this checklist to double-check everything:

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

 [ ✓ ] Visual Studio Code version = 1.74.0
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

RESULT: (0/18) checks failed
```
