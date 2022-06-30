---
title: VScode格式化配置
urlname: sm5cgu
date: 2020-11-06 14:00:00 +0800
tags: [VSCode]
categories: [其它]
---

### 一、安装常用插件：

Beautify、Eslint、Vetur、Prettier

<!-- more -->

### 二、setting.josn 配置

文件-首选项-设置-在 setting.josn 中编辑，打开这个 setting.josn 文件后将下面配置复制即可

```json
{
  // tab 大小为2个空格
  "editor.tabSize": 2,
  // 100 列后换行
  "editor.wordWrapColumn": 100,
  // 保存时格式化
  "editor.formatOnSave": true,
  // 开启 vscode 文件路径导航
  "breadcrumbs.enabled": true,
  // prettier 设置语句末尾不加分号
  "prettier.semi": false,
  // prettier 设置强制单引号
  "prettier.singleQuote": true,
  // 选择 vue 文件中 template 的格式化工具
  "vetur.format.defaultFormatter.html": "prettyhtml",
  // 显示 markdown 中英文切换时产生的特殊字符
  "editor.renderControlCharacters": true,
  // 设置 eslint 保存时自动修复
  "eslint.autoFixOnSave": true,
  // eslint 检测文件类型
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  // vetur 的自定义设置
  "vetur.format.defaultFormatterOptions": {
    "prettier": {
      "singleQuote": true,
      "semi": false
    }
  },
  // 修改500ms后自动保存
  "editor.formatOnSaveTimeout": 500,
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 500,
  "editor.codeActionsOnSaveTimeout": 500,
  "[javascript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  }
}
```

### 三、配置.editorconfig 文件

项目根目录下找到.editorconfig 这个文件，然后复制下面配置即可

```
# https://editorconfig.org
root = true                         # 根目录的配置文件，编辑器会由当前目录向上查找，如果找到 `roor = true` 的文件，则不再查找

[*]                                 # 匹配所有的文件
indent_style = space                # 空格缩进
indent_size = 4                     # 缩进空格为4个
end_of_line = lf                    # 文件换行符是 linux 的 `\n`
charset = utf-8                     # 文件编码是 utf-8
trim_trailing_whitespace = true     # 不保留行末的空格
insert_final_newline = true         # 文件末尾添加一个空行
curly_bracket_next_line = false     # 大括号不另起一行
spaces_around_operators = true      # 运算符两遍都有空格
indent_brace_style = 1tbs           # 条件语句格式是 1tbs

[*.js]                              # 对所有的 js 文件生效
quote_type = single                 # 字符串使用单引号

[*.{html,less,css,json}]            # 对所有 html, less, css, json 文件生效
quote_type = double                 # 字符串使用双引号

[package.json]                      # 对 package.json 生效
indent_size = 2                     # 使用2个空格缩进
```

### setting.json 参考

#### **setting.json 配置一**

\*\*

```json
{
  "editor.tabSize": 2,
  // "editor.formatOnPaste": true,
  "editor.acceptSuggestionOnCommitCharacter": false,

  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue-html",
    "vue",
    "html"
  ],
  /*  prettier的配置 */
  "prettier.printWidth": 120, // 超过最大值换行
  "prettier.tabWidth": 2, // 缩进字节数
  "prettier.useTabs": false, // 缩进使用tab
  "prettier.semi": false, // 句尾添加分号
  "prettier.singleQuote": true, // 使用单引号代替双引号
  "prettier.proseWrap": "preserve", // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
  "prettier.arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
  "prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
  "prettier.endOfLine": "auto", // 结尾是 \n \r \n\r auto
  "prettier.htmlWhitespaceSensitivity": "ignore",
  "prettier.ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
  "prettier.requireConfig": false, // Require a "prettierconfig" to format prettier
  "prettier.trailingComma": "none", // 在对象或数组最后一个元素后面是否加逗号

  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "files.associations": {
    "*.vue": "vue"
  },
  "window.openFilesInNewWindow": "default",
  "window.openFoldersInNewWindow": "on",
  "explorer.confirmDragAndDrop": false,
  "explorer.confirmDelete": false,
  "workbench.iconTheme": "vscode-icons",
  // 两者会在格式化js时冲突，所以需要关闭默认js格式化程序
  "javascript.format.enable": false,
  "typescript.format.enable": false,
  "vetur.format.defaultFormatter.html": "none",
  // js/ts程序用eslint，防止vetur中的prettier与eslint格式化冲突
  "vetur.format.defaultFormatter.js": "none",
  "vetur.format.defaultFormatter.ts": "none",
  "leetcode.endpoint": "leetcode-cn",
  "editor.codeActionsOnSave": null,
  "leetcode.workspaceFolder": "C:\\Users\\EDZ\\.leetcode",
  "leetcode.defaultLanguage": "javascript",
  "sync.gist": "此处是---sync.gist 替换为自己的----"
}
```

\*\*

#### **setting.json 配置二**

\*\*

```json
{
  // tab 大小为2个空格
  "editor.tabSize": 2,
  // 编辑器换行
  "editor.wordWrap": "off",
  // 保存时格式化
  "editor.formatOnSave": false,
  // 添加 vue 支持
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "wrap_attributes": "auto"
    },
    "prettyhtml": {
      "printWidth": 140,
      "singleQuote": false // 使用单引号代替双引号
    },
    "prettier": {
      "semi": false, // 句尾添加分号
      "singleQuote": true, // 使用单引号代替双引号
      "printWidth": 400, // 超过最大值换行
      "tabWidth": 2, // 缩进字节数
      "useTabs": false, // 缩进使用tab
      "proseWrap": "preserve", // // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
      "arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
      "bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
      "endOfLine": "auto", // 结尾是 \n \r \n\r auto
      "htmlWhitespaceSensitivity": "ignore",
      "ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
      "requireConfig": false, // Require a "prettierconfig" to format prettier
      "trailingComma": "none" // 在对象或数组最后一个元素后面是否加逗号
    }
  },
  // 选择 vue 文件中 template 的格式化工具
  // "vetur.format.defaultFormatter.html": "js-beautify-html",
  // "vetur.format.defaultFormatter.js": "vscode-typescript",
  "vetur.validation.template": true,
  /*  prettier的配置 */
  "prettier.printWidth": 400, // 超过最大值换行
  "prettier.tabWidth": 2, // 缩进字节数
  "prettier.useTabs": false, // 缩进使用tab
  "prettier.semi": false, // 句尾添加分号
  "prettier.singleQuote": true, // 使用单引号代替双引号
  "prettier.proseWrap": "preserve", // 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
  "prettier.arrowParens": "avoid", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
  "prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
  "prettier.endOfLine": "auto", // 结尾是 \n \r \n\r auto
  "prettier.htmlWhitespaceSensitivity": "ignore",
  "prettier.ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
  "prettier.requireConfig": false, // Require a "prettierconfig" to format prettier
  "prettier.trailingComma": "none", // 在对象或数组最后一个元素后面是否加逗号
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "octref.vetur"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "files.associations": {
    "*.vue": "vue",
    "*.cjson": "jsonc",
    "*.wxss": "css",
    "*.wxs": "javascript",
    "*.less": "less"
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue-html",
    "vue",
    "html"
  ],
  //让函数(名)和后面的括号之间加个空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": false,
  "emmet.includeLanguages": {
    "wxml": "html"
  },
  "minapp-vscode.disableAutoConfig": true,
  "files.autoSave": "onWindowChange",
  "workbench.iconTheme": "vscode-icons-mac",
  "leetcode.endpoint": "leetcode-cn",
  "leetcode.hint.configWebviewMarkdown": false,
  "leetcode.workspaceFolder": "/Users/ggzj/.leetcode",
  "leetcode.defaultLanguage": "javascript",
  "emmet.excludeLanguages": ["markdown"],
  "javascript.updateImportsOnFileMove.enabled": "never",
  "fileheader.customMade": {},
  "fileheader.cursorMode": {
    "description": "",
    "param": "value",
    "return": "{Any}"
  },
  "fileheader.configObj": {
    "createFileTime": true,
    "language": {
      "languagetest": {
        "head": "/$$",
        "middle": " $ @",
        "end": " $/"
      }
    },
    "autoAdd": false,
    "autoAddLine": 100,
    "autoAlready": true,
    "annotationStr": {
      "head": "/*",
      "middle": " * @",
      "end": " */",
      "use": false
    },
    "headInsertLine": {
      "php": 2
    },
    "beforeAnnotation": {
      "文件后缀": "该文件后缀的头部注释之前添加某些内容"
    },
    "afterAnnotation": {
      "文件后缀": "该文件后缀的头部注释之后添加某些内容"
    },
    "specialOptions": {
      "特殊字段": "自定义比如LastEditTime/LastEditors"
    },
    "switch": {
      "newlineAddAnnotation": true
    },
    "supportAutoLanguage": [],
    "prohibitAutoAdd": ["json"],
    "prohibitItemAutoAdd": [
      "项目的全称, 整个项目禁止自动添加头部注释, 可以使用快捷键添加"
    ],
    "moveCursor": true,
    "dateFormat": "YYYY-MM-DD HH:mm:ss",
    "atSymbol": "@",
    "atSymbolObj": {
      "文件后缀": "修改它的@符号"
    },
    "colon": ": ",
    "colonObj": {
      "文件后缀": "修改它的冒号"
    },
    "filePathColon": "路径分隔符替换",
    "showErrorMessage": false,
    "wideSame": false,
    "wideNum": 13,
    "CheckFileChange": false,
    "createHeader": true,
    "useWorker": false,
    "designAddHead": false,
    "typeParam": true
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```
