# VS Code Setup Instructions

Instructions for setting up VS Code like the EDA computers

> Note: These instructions have you install ESLint globally. The ESLint documentation rightfully recommends against this in lieu of installing ESLint locally on a per project/repository basis. We prefer a global install so we don't need to manage ESLint dependencies on each challenge/exercise repo. There isn't really any harm in having a global install because if you have an `.eslintrc` file in your project, it will override your global one. If you do this, don't forget to include ESLint and all of its plugins in your project's `package.json`.

Follow these steps in order.

## 1. Install npm packages

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

> Note: At the moment we're installing specific versions of these packages because we experienced a lot of incompatibilities when using the latest version of them all. Hopefully we'll be able to go back to the latest versions of everything at some point in the future.


## 2. Install VS Code extensions

  * ESLint
  * vscode-icons (optional, but pretty :wink:)


## 3. Edit VS Code's User Settings

File :arrow_right: Preferences :arrow_right: Settings

```js
{
  "editor.tabSize": 2,
  "workbench.iconTheme": "vscode-icons"
}
```

> Note: The `workbench.iconTheme` will already be there if you've _activated_ vscode-icons.


## 4. Edit VS Code's `keybindings.json`

This enables `CMD+b` to automatically fix as many linting errors/warnings as it can with a single shortcut. You might need to use a different shortcut if you're on MacOS or Windows. If `cmd+b` doesn't work for you, try `meta+b` (meta is the Alt key) or `ctrl+shift+b`.

File :arrow_right: Preferences :arrow_right: Keyboard Shortcuts and then click on the icon in the top-right to _Open Keyboard Shortcuts (JSON)_. Use this as the contents.

```js
[
  {
    "key": "cmd+b",
    "command": "eslint.executeAutofix",
    "when": "editorFocus"
  }
]
```

## 5. Create `~/.eslintrc.json`

> Note: `~` is your _home_ folder (usually the folder you start in when you open your terminal).

```json
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
  }
}
```

## More Notes

* The EDA computers also install [`git-iam`](https://npmjs.com/package/git-iam) globally, but there is no need to install this on personal laptops/computers.
