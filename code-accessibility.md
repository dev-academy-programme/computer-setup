# Accessibility of code in VS Code or the terminal

You may find that some minor adjustments to your coding setup can have a huge impact on the effectiveness and enjoyability of your experience. Here are a few things you might try customising to suit your own needs. While they are largely sourced from resources focusing on dyslexia, anyone can benefit from them.

Further accessibility features, such as keyboard navigation, screenreader compatibility, and audio cues are described in the [VS Code docs](https://code.visualstudio.com/docs/editor/accessibility).

Additionally, if you need hands-free coding, read [Josh W. Comeau's description of how he does this](https://www.joshwcomeau.com/blog/hands-free-coding/).

## Customisations that might improve readability:
- Colours (themes and contrast)
  * [This one](https://github.com/SpeedyLom/dislexic-vscode) was designed by a dyslexic coder for their own use
- Typefaces
  * [This one](https://www.opendyslexic.org/) was specifically designed for dyslexic readers and comes in a "monospace" font, which is what is typically used in code editors
- Font size, letter spacing, line spacing [(general guidance)](https://www.bdadyslexia.org.uk/advice/employers/creating-a-dyslexia-friendly-workplace/dyslexia-friendly-style-guide)
  * The editor is highly customisable within VS Code, and so is the integrated terminal. For example, using `"terminal.integrated.fontSize": 16` in your settings.json for VS Code. The markdown preview can also be customized with `"markdown.preview.lineHeight"`, `"markdown.preview.fontSize"`, and `"markdown.preview.fontFamily"`.
- Magnification of entire interfaces (i.e. zooming in/out)
  * `"window.zoomLevel": 1.5` (or whatever value you choose) in your settings.json for VS Code
- [Icons for different file types](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)
- Hiding whitespace characters
  * ```
    "editor.renderWhitespace": "none",
    "editor.renderControlCharacters": false
    ```
    in your settings.json for VS Code

## Customisations that might benefit focus and attention:
- Hiding the minimap
  * `"editor.minimap.enabled": false` in your settings.json for VS Code
- Colours to highlight (or de-emphasise) certain panels, applications, or work spaces

## Customisations that might reduce errors:
- [Spellchecker for code](https://marketplace.visualstudio.com/items?itemName=streetsIDEsoftware.code-spell-checker)
- [Code snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets)
- Auto-completion & [auto-closing](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag) of tags
- [Closure highlighting](https://marketplace.visualstudio.com/items?itemName=Durzn.brackethighlighter)

## Resources
These articles are primarily geared towards dyslexia, but include suggestions that may also be useful for people who are neurodivergent.
* [Personalising VSCode for Dyslexia](https://jenn-hall.medium.com/personalising-vscode-for-dyslexia-60aac1a36b4d)
* [Have Dyslexia? Make Coding Easier In Visual Studio Code](https://dev.to/deadlybyte/have-dyslexia-make-coding-easier-in-visual-studio-code-4kmg)
* [Top tips for teaching people with dyslexia to code](https://blog.decoded.com/top-tips-for-teaching-people-with-dyslexia-to-code-ffc604668fe8)
* [Dyslexia and Coding](https://datacarpentry.org/blog/2017/09/coding-and-dyslexia)
