# Template Literal Editor for VS Code

Open ES6 template literals and other configurable multi-line strings in any language in a synced editor, with language support (HTML, CSS, SQL, shell etc).

Instructions:
- Install extension from VS Code Marketplace
- Open JavaScript or TypeScript file, or a file in some other language if customized via "templateLiteralEditor.regexes" configuration
- Place cursor inside any template literal string and press Ctrl+Enter or Ctrl+Shift+Enter
- Select language (defaults to html)
- Outermost template literal range opens in the selected language in a side-by-side view, synced with the original.
- Enjoy syntax highlighting, completions, formatting, comments, snippets, your preferred editing extensions, etc.
- Close views by pressing Ctrl+Backspace or Ctrl+Shift+Backspace, or via "Revert And Close Editor" action, to avoid unnecessary save dialogs.
- When you edit the original document the template editor is kept in sync, but if template boundaries are touched or a sync error happens, then the editor is closed or becomes tainted for safety and can be closed or reused.

## Releases

See [changelog](https://marketplace.visualstudio.com/items/plievone.vscode-template-literal-editor/changelog).

## Notes

This proof-of-concept uses only public extension APIs and could be made quite a bit more polished if implemented in VS Code proper. There may be unknown edge cases and plugin interactions, but it seems to already be a helpful tool for reading and editing templates in different languages.

Note that creating and closing files in multiple editors is a bit poorly supported in extensions. Internally, Ctrl+Enter currently creates untitled documents. They are refocused, reverted and closed when commanded, which causes tab flicker. Ctrl+Shift+Enter creates unsaved documents with filenames, which are faster to reuse and easier to identify in tabs, but which may be broken on Windows (according to a bug report on an earlier version). When tainted, they are replaced with a placeholder string so that VS Code can reuse them for templates in the same document and language. Undo works as usual, with a caveat that undo history includes the initial empty document and earlier templates, if reused. Reloading VS Code results in stale template editors, as storing, iterating, detecting and closing stale editors can be error prone, so it is currently unimplemented, sorry about that. Would also like to have sensible tab names, sync viewports and cursors when requested, and proxy saving of the template to saving of the original document, but those cannot be accomplished via the current extension API.

Developed on VS Code 1.11.0-insider on MacOS, as a personal tool, hoping something similar could be implemented properly with VS Code internals. Multiline strings could be focused in on many different host languages, in addition to JS and TS. It could be implemented as a side-by-side view similar to diff view, or as dimmed/highlighted layers on top of each other. For simplicity, the mapping is currently character-to-character, including line breaks. Also simple transformations, such as stripping whitespace and autoformatting, could be live mapped, but they may be best left for build tools and extensions working with template literals and template languages in question, and focus the feature on character-to-character document parts only.

MIT License. Feel free to use the code however you wish. You can find the code inside the extension package, TypeScript sources are included. There is no repo yet, but you can send your greetings and bug reports via Q & A and User Reviews on [Code Marketplace](https://marketplace.visualstudio.com/items?itemName=plievone.vscode-template-literal-editor).
