> 配置

![image](https://github.com/htllog/StudyNotes/assets/118370026/22b476f0-dd42-4d7a-b733-cfdaaa83cf33)




> setting.json 文件配置

```json
{
  // vscode默认启用了根据文件类型自动设置tabsize的选项
  "editor.detectIndentation": false,
  // 重新设定tabsize
  "editor.tabSize": 2,
  // #每次保存的时候自动格式化
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  // #每次保存的时候将代码按eslint格式进行修复
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  //  #去掉代码结尾的分号
  "prettier.semi": true,
  //  #使用单引号替代双引号
  "prettier.singleQuote": false,
  //  #让函数(名)和后面的括号之间加个空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  "javascript.format.semicolons": "insert",
  // #缩进字节数
  "prettier.tabWidth": 2,
  // #缩进不使用tab，使用空格
  "prettier.useTabs": false,
  // #在对象，数组括号与文字之间加空格
  "prettier.bracketSpacing": true,
  // #末尾空行
  "prettier.endOfLine": "auto",
  "prettier.htmlWhitespaceSensitivity": "ignore",
  // #在jsx中使用单引号代替双引号
  "prettier.jsxSingleQuote": false,
  "prettier.requireConfig": false,
  "prettier.trailingComma": "es5",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "[typescript]": {
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": false
    }
  },
  // 字体大小
  "editor.fontSize": 12,
  "explorer.confirmDelete": false,
  "editor.fontLigatures": false,
  "security.workspace.trust.untrustedFiles": "open",
  "editor.minimap.enabled": false,
  "javascript.updateImportsOnFileMove.enabled": "always"
}
```

