# 关于eslint 和项目中开发代码规范化

当一个项目多个人开发的时候，就会出现“百花齐放”的局面。小到缩进是Tab是2个字符还是4个字符，单引号和双引号...之类的编写方式。大到变量为定义使用，重复定义等情况。为了代码质量高，阅读性好，后期易维护。团队使用了eslint。基于airbnb制定的规则。


### eslint + eslint-friendly-formatter 统一js代码编写规范
1. `yarn add` 安装 `eslint`, `eslint-friendly-formatter`, `babel-eslint`,`eslint-config-airbnb` 等一系列依赖包(目前项目中已经写入 `package.json`中，直接 `yarn`就好了)

2. 在开发编辑工具中安装 `eslint`
3. 工具配置 以vscode为例，点开 文件 -> 首选项 -> 用户设置 (目前没啥需要再次设置，当然也可以根据自己喜好进行一些配置，例如保存自动格式化`eslint.autoFixOnSave`。)

4. 可以再次配置`eslint rules`。现以店铺图业务模块进行了通用的配置(如下`.eslintrc`文件)。后续根据具体业务可以添加或调整。(可以参考 
- [eslint官网](https://eslint.org/)
- [https://github.com/yannickcr/eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)
- [https://github.com/evcohen/eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y)
- [https://github.com/benmosher/eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import)

``` javascript
.eslintrc

{
  "env": {
    "browser": true
  },
  "parser": "babel-eslint",
  "extends": "airbnb",
  "rules": {
    // generator 的 * 前后空格使用规则
    "generator-star-spacing": [0],
    // 禁止函数在不同条件下返回不同类型的值
    // @off 有时候会希望通过参数获取不同类型的返回值
    "consistent-return": [0],
    // 禁止prop类型 @off prop可以为任何类型
    "react/forbid-prop-types": [0],
    // react文件扩展名 不仅仅是 .jsx 允许 .js
    "react/jsx-filename-extension": [1, {
      "extensions": [".js"]
    }],
    // require 必须在全局作用域下
    "global-require": [1],
    // import 更倾向default @off
    "import/prefer-default-export": [0],
    // bind 
    "react/jsx-no-bind": [0],
    // prop类型
    "react/prop-types": [0],
    // 最优组件无state 设置 @off
    "react/prefer-stateless-function": [0],
    // 禁止出现 if (cond) { return a } else { return b }，应该写为 if (cond) { return a } return b
    // @off 有时前一种写法更清晰易懂
    "no-else-return": [0],
    // 禁止使用特定的语法 @off
    "no-restricted-syntax": [0],
    // import 规定， 禁止import没在package中的依赖
    "import/no-extraneous-dependencies": [0],
    // 禁止在变量被定义之前使用它
    "no-use-before-define": [0],
    // 禁止 静态element
    "jsx-a11y/no-static-element-interactions": [0],
    // 禁止嵌套的三元表达式
    "no-nested-ternary": [0],
    // 箭头函数的书写规则
    "arrow-body-style": [0],
    // import 扩展
    "import/extensions": [0],
    // 禁止位运算
    "no-bitwise": [0],
    // 禁止在 if、for、while 中出现赋值语句，除非用圆括号括起来
    "no-cond-assign": [0],
    // import 禁止不明确模块和文件
    "import/no-unresolved": [0],
    // generator 函数内必须有 yield
    "require-yield": [1],
    // 换行符使用规则
    "linebreak-style": ["error", "windows"],
    // obj = { a: a } 必须转换成 obj = { a } 
    "object-shorthand": ["error", "always"],
    // arr = [ 1, 2, 3 ]必须转化成 arr = [1,2,3] 都可自动格式化
    "array-bracket-spacing": ["error", "never"],
    // 定义对象的花括号前后是否要加空格
    "object-curly-spacing": ["error", "always"],
    // 禁止变量名中使用下划线
    "no-underscore-dangle": ["error", {"allow": ["_this"]}],
    // 禁止 console
    "no-console": ["error", {"allow": ["log", "warn", "error"]}],
    // 必须使用解构
    "prefer-destructuring": ["error", {"object": true}],
    // 所有锚点是有效的， 例如 <a>标签必须有href
    // 这里设置了<Link>标签link为 to是允许的
    "jsx-a11y/anchor-is-valid": ["error",
      {
        "components": ["Link"],
        "specialLink": ["to"],
        "aspects": ["noHref", "invalidHref", "preferButton"]
      }
    ],
    // 禁止出现无用的表达式
    "no-unused-expressions": [2, { "allowTernary": true }],
    // 点击事件后还需要有其他键盘事件 @off 
    "jsx-a11y/click-events-have-key-events": 0,
    // react中属性属性顺序 @off
    "react/sort-comp": 0,
    // 数组的 map、filter、sort 等方法，回调函数必须有返回值
    "array-callback-return": ["error", { "allowImplicit": false }],
    // 所有的箭头函数有回调
    "prefer-arrow-callback": 0,
    // 执行JSX多行元素的关闭括号位置。
    "react/jsx-closing-bracket-location": 0,
    // 强制多行JSX元素的结束标记位置
    "react/jsx-closing-tag-location": 0,
    // 禁止在return中赋值
    "no-return-assign": 0,
    // 限制单行代码的长度 @off
    "max-len": 0,
    // react 循环中key 禁止使用 index, 确实建议这样做，
    // 但这个规则 连 `pro_${index}_xx` 这种都是禁止的，由于数据返回有// 时候没有 唯一表示，所以建议用组装 字符串+index 来做key
    "react/no-array-index-key": 0
  },
  "parserOptions": {
    "ecmaFeatures": {
      "experimentalObjectRestSpread": true
    }
  }
}


```

*说明：off = 0 ; 1= warning; 2 = error*

5. 在`package.json`中的`scripts`中配置
```
"lint": "eslint --fix --format node_modules/eslint-friendly-formatter src/config/ src/routes/erp/project/ src/services/erp/project/"
```
`eslint-friendly-formatter` 可以配置多个文件夹进行检查
参照 [eslint-friendly-formatter使用](https://github.com/royriojas/eslint-friendly-formatter)

6. 在项目根目录中执行 `npm run lint`根据提示一一更正不合规范代码。

7. 哪个模块不想进行`eslint`检测则可以在根目录下`.eslintignore`文件中配置相应文件
### eslint + prettier

> 现在流行的一种，但坑太多了。感兴趣的可以玩一波。欢迎一起讨论。

一开始想使用eslint + Prettier(vscode编辑器上的插件) 偷懒，在保存的时候就直接格式化代码了。语法类的编辑器也有提示。捣鼓了2个晚上，晚上在家用os系统，然后白天在公司用win系统。切换时，没那么顺利，只在家里的mac配置成功了。(例如: eslint 中的`linebreak-style`不同，vscode配置项一样也会出现问题。后面有时间再试一试)



关于eslint + prettier保存自动配置的，网络上很多文件。我在mac中vscode 编辑器 配置的文件如下。

```js
{
    "workbench.colorTheme": "Visual Studio Dark",
    "editor.fontSize": 14,
    "editor.tabSize": 2,
    "editor.wordWrap": "wordWrapColumn",
    "javascript.format.insertSpaceAfterKeywordsInControlFlowStatements": true,
    "prettier.eslintIntegration": true,
    "eslint.trace.server": "messages",
    "team.showWelcomeMessage": false,
    "prettier.singleQuote": true,
    "prettier.semi": false,
    "eslint.autoFixOnSave": true,
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        {
            "language": "react",
            "autoFix": true
        }
    ],
    "prettier.trailingComma": "all"
}
```
eslint 配置如下

``` js
{
  "env": {
    "browser": true
  },
  "parser": "babel-eslint",
  "extends": "airbnb",
  "rules": {
    "generator-star-spacing": [0],
    "consistent-return": [0],
    "react/forbid-prop-types": [0],
    "react/jsx-filename-extension": [1, { "extensions": [".js"] }],
    "global-require": [1],
    "import/prefer-default-export": [0],
    "react/jsx-no-bind": [0],
    "react/prop-types": [0],
    "react/prefer-stateless-function": [0],
    "no-else-return": [0],
    "no-restricted-syntax": [0],
    "import/no-extraneous-dependencies": [0],
    "no-use-before-define": [0],
    "jsx-a11y/no-static-element-interactions": [0],
    "no-nested-ternary": [0],
    "arrow-body-style": [0],
    "import/extensions": [0],
    "no-bitwise": [0],
    "no-cond-assign": [0],
    "import/no-unresolved": [0],
    "require-yield": [1],
    "camelcase": "off", 
    "no-new": "off", 
    "space-before-function-paren": "off", 
    "no-plusplus": "off", 
    "max-len": "off", 
    "func-names": "off", 
    "no-param-reassign": "off",
    "no-underscore-dangle": ["error", { "allow": ["_this"] }],
    "no-console": ["error", {"allow": ["log", "warn", "error"] }]
  },
  "parserOptions": {
    "ecmaFeatures": {
      "experimentalObjectRestSpread": true
    }
  }
}
```
保存的时候回直接格式化文件。例如`{}`Object中留空格，Array中不留，语句后自动添加 `;`，单双引号等等。主要是开启了 `eslint.autoFixOnSave`。有兴趣的可以试试。














































