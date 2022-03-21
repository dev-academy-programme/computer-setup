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
