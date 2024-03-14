# Ubuntu Computer Setup for EDA

**Pro-tip:** There's a copy button hidden in the top-right of each code block for easy copy/pasting.

![Screen Shot of clicking on the copy button](https://user-images.githubusercontent.com/47387/161153831-7a3ca544-0ad2-4977-aec8-92436f1a6bc5.png)

**Pro-tip #2:** You might need to re-login to see the effects of changing your shell

## 1. Installing the essentials

First run this in your terminal to update your sources:

```sh
sudo apt-get update
```

Then run this to install the packages we need:

```sh
sudo apt-get install build-essential python-is-python3 zsh
```

Run this command to set `zsh` as your default shell:

```sh
chsh -s $(which zsh)
```

Close and open a new terminal and if you are prompted to make a choice, choose `q`.

### 1.1 Installing oh-my-zsh

We're going to install `oh-my-zsh` to make your terminal/shell experience a bit more pleasant.

> _Oh My Zsh_ is a delightful, open source, community-driven framework for
> managing your Zsh configuration. It comes bundled with thousands of helpful
> functions, helpers, plugins, themes, and a few things that make you shout...

Copy this command into your terminal:

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 1.2 Configuring ZSH

Zsh installs a command `omz` to configure itself. To set your theme to "bira" run:

```sh
omz theme set bira
```

Close your terminal and open a new one.

## 2. Preparing build tools

## 3. Setup Visual Studio Code

Install _Visual Studio Code_ if it isn't already installed.

https://code.visualstudio.com/download

Then in your terminal, open _VS Code_ with:

```
code .
```

### 3.1 Installing extensions

Install the following _VS Code_ extensions:

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

### 3.2 Visual Studio Code settings

In _VS Code_:

1. Click the _Settings_ cog button in the bottom left and open the _Command Palette_.
2. Type `settings.json` into the little search box that appears at the top of your screen.
3. Click on the `Preferences: Open User Settings (JSON)` option to open your `settings.json` config file.

Paste this content after the first line (`{`):

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

### 3.3 Make VS Code your default Git editor

Run this command in your terminal:

```sh
git config --global core.editor "code --wait"
```
Run this command to force git to always use ssh urls for github.com

```sh
git config --global url.'git@github.com:'.insteadof https://github.com/
```

### 4.4 Install NVM

NVM is a tool to install and manage NodeJS versions.

First, check if you have installed NVM before:

```sh
type nvm
```

If you see something like `nvm is a shell function from /home/username/.nvm/nvm.sh` you've already installed NVM and can go to section 4.5, if you see a `nvm not found` message then keep reading.

First, check if you have NodeJS installed:

```sh
which node
```

If it logs "node not found", that's perfect. We want NVM to manage node and npm on our dev machine.

If it logs anything else (e.g. `/usr/bin/node`), you need to uninstall NodeJS, this differs depending on
how you installed it. Contact a facilitator if you get stuck.

Enter this command into your terminal to download and install NVM:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

This command will initialise NVM when you open a terminal:

```sh
cat << 'EOF' >> ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
EOF
```

Now run this command to reload your `~/.zshrc`:

```sh
omz reload
```

### 4.5 Installing Node and NPM with NVM

Install the latest "Long Term Support" (i.e. very stable) version of node:

Run this command in your terminal:

```sh
nvm install --lts
```

Then, also in your terminal, run:

```sh
nvm alias default node
```

To confirm, run this command (_We're expecting something in the `v18.x` range_):

```sh
nvm current
```

### 4.6 Building sqlite3

`sqlite3` is a database package that we use a lot during bootcamp. At this point
you should be set up with everything you need to build it.

Run this command to confirm:

```sh
npx --yes @donothing/can-u-build-sqlite3
```

If it succeeds it will log `Everything looks good`.

## 6. Cloning your first repo

We're going to clone a repo to make sure everything is working fine.

We'll start by creating a directory to keep all your repos in (_it doesn't really matter what you call it_):

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

You'll need to configure `git` to know your name...

```sh
git config --global user.name "Firstname Lastname"
```

... and your email address. These will be recorded as the author in commits you make:

```sh
git config --global user.email "your.name@example.com"
```

From your Ubuntu terminal, clone down `clone-a-repo-test`:

```sh
git clone git@github.com:dev-academy-foundations/clone-a-repo-test.git
```

Now we're going to change into the new directory:

```sh
cd clone-a-repo-test
```

and open _Visual Studio Code_:

```
code .
```

Now you should be looking at the `clone-a-repo-test` in your editor. Click on the `README.md` file to read the hidden message.

Run this command in your terminal:

```sh
open .
```

Your file browser should open that directory.

## 7. You're all set up

Run this checklist to double-check everything:

```sh
npx --yes @devacademy/checklist
```

You should see something like this (_all ticks, no crosses, 0/x failed_):

```
Shell environment:

 [ ✓ ] linux
 [ ✓ ] $SHELL = /bin/zsh
 [ ✓ ] ZSH version = zsh 5.9

Node setup:

 [ ✓ ] /Users/gerard/.nvm exists
 [ ✓ ] NVM config found in ~/.zshrc
 [ ✓ ] Node version = v18.12.1
 [ ✓ ] NPM version = 9.2.0

Visual studio code:

 [ ✓ ] Visual Studio Code version = 1.70.2
 [ ✓ ] Git editor is code --wait
 [ ✓ ] VSCode extension 'dbaeumer.vscode-eslint' installed
 [ ✓ ] VSCode extension 'esbenp.prettier-vscode' installed
 [ ✓ ] VSCode extension 'ms-vsliveshare.vsliveshare' installed
 [ ✓ ] VSCode extension 'eamodio.gitlens' installed
 [ ✓ ] VSCode extension 'vscode-icons-team.vscode-icons' installed

Build requirements (for node-gyp):

 [ ✓ ] Git version = git version 2.37.2
 [ ✓ ] Found cc = /usr/bin/cc
 [ ✓ ] Found make = /usr/bin/make
 [ ✓ ] Found python version: Python 3.10.4 at /usr/bin/python3

RESULT: (0/21) checks failed
```
