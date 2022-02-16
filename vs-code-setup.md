# VS Code Setup Instructions

> Instructions for setting up VS Code like the EDA computers

Follow these steps in order.

## 1. Install VS Code extensions

  * [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
  * [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
  * WSL - Remote (Windows 10 only)
  * [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare) (online students only)
  * [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) (optional, but recommended)
  * [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons) (optional, but pretty :wink:)

## 2. Edit VS Code's User Settings

File :arrow_right: Preferences :arrow_right: Settings

```json
{
  "editor.tabSize": 2,
  "workbench.iconTheme": "vscode-icons",
  "editor.codeActionsOnSave": { "source.fixAll.eslint": true },
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs":"active"
  "[javascript]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.semi": false,
  "prettier.singleQuote": true
}
```

> Note: The `workbench.iconTheme` will already be there if you've _activated_ vscode-icons.

## More Notes

* The EDA computers also globally install [`git-iam`](https://npmjs.com/package/git-iam), but there is no need to install this on personal laptops/computers (only shared computers).

## Contributing

If you find some of these instructions too ambiguous or hard to follow, we would love it if you issued a pull request to improve this doc. :heart:

