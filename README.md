# VS Code extension: SQL tagged template literals

A VS Code extension, which enables syntax highlighting for template literals tagged with an `sql` function in JavaScript and TypeScript files. That function can be defined as follows:

`const sql = String.raw;`

There are 2 version of the extension available:

- [extension](./extension): Syntax highlighting, syntax validation, type checking and formatting
- [extension-syntax-only](./extension-syntax-only): Syntax highlighting only

![GIF of code snippet showing SQL syntax](./docs/preview.gif)

## Interesting bits about the grammar

- To debug the grammer, check the language scopes using the - `Developer: Inspect Editor Tokens and Scopes` command.
- If `patterns[].name` does not start with `string.js`, the code matched by the pattern (beginning and end) are not highlighted as JavaScript code.
- The keys in `beginCaptures` and `endCaptures` refer to the capture groups in the corresponding regex, where `0` refers to the entire match. By assigning capture groups a name, they can be highlighted accordingly. E.g. using `entity.name.function.ts` makes the string matched by the capture group be highlighted as a function name.
- The pattern `source.ts#template-substitution-element` refers to the variables in the template literal, e.g. `${userId}`. It should probably be the first pattern in the list to ensure these are highlighted properly.
- The pattern `source.ts#string-character-escape` refers to escaped backtics in the template literal, e.g. `\``. It should come before the SQL patterns in the list in order to not confuse SQL.
- A pattern cannot span multiple lines. To match something across multiple lines, you have to use nested patterns; [good explanation](https://github.com/Microsoft/vscode-textmate/issues/41#issuecomment-358459018).
- The [TypeScript language definition](https://github.com/microsoft/vscode/blob/main/extensions/typescript-basics/syntaxes/TypeScript.tmLanguage.json) can be useful to lookup and use existing patterns. The `sql()` function syntax in this repository makes use of some of the patterns used in a `#function-call`.

## Publish new version

_(For maintainers)_

1. Update version in package.json and CHANGELOG.md

2. Publish new version

   ```
   npm install -g vsce

   REPO=https://github.com/frigus02/vscode-sql-tagged-template-literals/raw/main/

   cd extension/
   vsce publish --baseImagesUrl $REPO/extension/

   cd extension-syntax-only/
   vsce publish --baseImagesUrl $REPO/extension-syntax-only/
   ```

3. Tag new version

   ```
   git tag extension-vX.X.X
   git tag extension-syntax-only-vX.X.X
   ```

## Thanks

This is based on several great existing extensions:

- https://github.com/mjbvz/vscode-comment-tagged-templates
- https://github.com/ForbesLindesay/vscode-sql-template-literal
- https://github.com/Ladeiras/vscode-sql-template-literal
