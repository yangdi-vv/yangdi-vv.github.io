---
title: tslint.js 详解
categories:
- TypeScript 
tags:
- 笔记
---

# tslint.js 详解

## 一、区别
- eslint 是用来检测 javascript 的代码错误及代码规范问题，原理是通过对AST的分析完成。
- tslint 是用来检测 typescript 的代码错误及代码规范问题，原理也是通过对AST的分析完成。
- eslint 和 tslint 都是基于不同parser对AST解析。

## 二、AST
- AST是对源代码的抽象语法结构树状表现形式，并不是完整的语法结构（CST）：
![AST](https://tva1.sinaimg.cn/large/00760ex2ly8h1m3lxrfd6j308v06nq35.jpg)
- AST有不同的标准存在，最早用来解析代码成AST的 parser 是 esprima，它参考了 Mozilla 浏览器的 SpiderMonkey 引擎的 AST 的标准。
- AST标准大体分为estree(esprima、espree、babel parser（babylon）、acorn)和非estree(typescript、terser)
- ![AST标准](https://tva4.sinaimg.cn/large/00760ex2ly8h1lz2opzsyj30k008w3yo.jpg)

## 三、eslint - parser
- eslint 默认使用parser的是 espree, 同时也支持parser的切换：
    1. 默认的espree
    ```
    {
        "parserOptions": {
            "ecmaVersion": 6,
            "sourceType": "module",
            "ecmaFeatures": {
                "jsx": true
            }
        }
    }
    ```
    2. 通过 @babel/eslint-parser 来切换到babel 的AST，babel的parser支持的语法更多，再babel7之后也支持TS及jsx、flow语法解析。
  
    ```
    {
        parser: "@babel/eslint-parser",
        parserOptions: {
            sourceType: "module",
            plugins: []
        },
    }
    ```
    3. 可以通过@typescript/parser来切换到typescript的parser
    ```
    {
      "parser": "@typescript-eslint/parser",
      "parserOptions": {
          "project": "./tsconfig.json"
      }
    }
    ```
  4. 可以通过 vue-eslint-parser 来解析 vue 的单文件组件，因为 vue 组件代码同样通过 eslint 来检查规范和逻辑错误，所以实现了对应的 parser
  ```
  {
         "parser": "vue-eslint-parser",
         "parserOptions": {
             "sourceType": "module",
             "ecmaVersion": 2018,
             "ecmaFeatures": {
                 "globalReturn": false,
                 "impliedStrict": false,
                 "jsx": false
             }
         }
     }
  ```

## 四、eslint - 参数
- extends
> 内置配置预设的名称（请参阅下面的内置预设），或由此配置扩展的其他配置文件的路径或路径数组。该值使用节点模块解析语义处理。例如，值"tslint-config"将告诉 TSLint 尝试将名为“tslint-config”的模块的主文件加载为配置文件。也可以指定节点模块内的特定文件，例如。"tslint-config/path/to/submodule". 还支持 JSON 文件或 JS 模块的相对路径，例如"./tslint-config".

- rulesDirectory
> [自定义规则][2] 的目录路径或目录路径数组。index.js如果将 an放在您的规则目录中，则使用节点模块解析语义处理这些值。如果无法解析模块，我们将使用相对或绝对路径。如果您想避免模块解析，您可以直接使用相对或绝对路径（例如 with ./）。

- rules
```
{
	"defaultSeverity": "error",
	"extends": [
		"tslint:recommended"
	],
	"rules": {
        "member-access": true, // 设置成员对象的访问权限（public,private,protect)
        "member-ordering": [// 设置修饰符顺序
            true,
            {
                "order": [ 
                    "public-static-field",
                    "public-static-method",
                    "protected-static-field",
                    "protected-static-method",
                    "private-static-field",
                    "private-static-method",
                    "public-instance-field",
                    "protected-instance-field",
                    "private-instance-field",
                    "public-constructor",
                    "protected-constructor",
                    "private-constructor",
                    "public-instance-method",
                    "protected-instance-method",
                    "private-instance-method"
                ]
            }
        ],
        // "no-empty-interface":true,// 不允许空接口
        "no-parameter-reassignment":false,// 不允许修改方法输入参数
        "prefer-for-of":true,// 如果for循环中没有使用索引，建议是使用for-of
 
        // 功能特性
        "no-namespace":false,
        "only-arrow-functions":false, //禁止使用传统（非箭头）函数表达式
        "no-shadowed-variable": true, // 不允许子作用域与外层作用域声明同名变量
        "no-string-literal":false,
        "ban-types": false,// 禁止内置原始类型
        "await-promise":true,// 不允许没有Promise的情况下使用await
        "curly":true,// if/for/do/while强制使用大括号
        "forin":false,// 使用for in语句时，强制进行hasOwnProperty检查
        "no-arg":true,// 不允许使用arguments.callee
        "no-bitwise":false, // 不允许使用特殊运算符 &, &=, |, |=, ^, ^=, <<, <<=, >>, >>=, >>>, >>>=, ~
        "no-conditional-assignment":true,// do while/for/if/while 语句中将会对例如if(a=b)进行检查
        "no-console":true,// 不允许使用console对象
        "no-debugger":true,// 不允许使用debugger
        "no-duplicate-super":true,// 不允许super() 两次使用在构造函数中
        "no-empty":false,// 函数体不允许空
        "no-eval":true,// 不允许使用eval
        "no-for-in-array":true,// 不允许对Array使用for-in
        "no-invalid-template-strings":true,// 只允许在模板字符串中使用${
        // "no-invalid-this":true,// 不允许在class之外使用this
        // "no-null-keyword":true,// 不允许使用null,使用undefined代替null，指代空指针对象
        "no-sparse-arrays":true,// 不允许array中有空元素
        "no-string-throw":true,// 不允许throw一个字符串
        "no-switch-case-fall-through":true,// 不允许case段落中在没有使用breack的情况下，在新启一段case逻辑
        "no-unsafe-finally":true,// 不允许在finally语句中使用return/continue/break/throw
        "no-unused-expression":true,// 不允许使用未使用的表达式
        "no-use-before-declare":true,// 在使用前必须声明
        "no-var-keyword":true,// 不允许使用var
        "radix":false,// parseInt时，必须输入radix精度参数
        // "restrict-plus-operands":true,// 不允许自动类型转换，如果已设置不允许使用关键字var该设置无效
        "triple-equals":false,// 必须使用恒等号，进行等于比较
        "use-isnan":true,// 只允许使用isNaN方法检查数字是否有效
 
        // 维护性功能
        "indent":[true, "spaces", 4],// 每行开始以4个空格符开始
        "max-classes-per-file":[true,1],// 每个文件中可定义类的个数
        "max-file-line-count":[true,1000],// 定义每个文件代码行数
        "max-line-length":[true,300],// 定义每行代码数
        "no-default-export":true,// 禁止使用export default关键字，因为当export对象名称发生变化时，需要修改import中的对象名。https://github.com/palantir/tslint/issues/1182#issue-151780453
        "no-duplicate-imports":true,// 禁止在一个文件内，多次引用同一module
 
        // 格式
        "align":[true,"parameters","arguments","statements","members","elements"],// 定义对齐风格
        "array-type":[true,"array"],// 建议使用T[]方式声明一个数组对象
        "class-name":false,// 类名以大驼峰格式命名
        "comment-format":[true, "check-space"],// 定义注释格式
        "encoding":false,// 定义编码格式默认utf-8
        "import-spacing":true,// import关键字后加空格
        "interface-name":[true,"always-prefix"],// interface必须以I开头
        "jsdoc-format":false,// 注释基于jsdoc风格
        "new-parens":true,// 调用构造函数时需要用括号
        "object-literal-sort-keys":false,
        "no-consecutive-blank-lines":[true,2],// 不允许有空行
        // "no-trailing-whitespace": [// 不允许空格结尾
        //     true,
        //     "ignore-comments",
        //     "ignore-jsdoc"
        // ],
        "no-unnecessary-initializer":true,// 不允许没有必要的初始化
        "variable-name":[false, "check-format",// 定义变量命名规则
            "allow-leading-underscore",
            "allow-trailing-underscore",
            "ban-keywords"]
    },
	"rulesDirectory": [],
	"linterOptions": {
		"exclude": [
			"e2e/**/*"
		]
	}
}
```


## 五、eslint 和 tslint的融合
- 首先通过 @typescript-eslint/parser 将tslint的AST转化为estree的AST，eslint无法检测的ts属性会再次使用tslint进行检测

## 六、总结
js 有不同的 parser，分为 estree 系列和非 estree 系列：

- estree 系列有 esprima、acorn 以及扩展自 acorn 的 espree、babel parser 等。
- 非 estree 系列有 typescript、terser 等。 

eslint 中支持了 parser 的切换，可以在 babel parser、vue template parser、typescript 和 espree 中切换，当然也可以扩展其他的 parser。

tslint 是基于 typescript 做 parse 的一个独立的工具。它和 eslint 都是基于 AST 检查代码中的逻辑和格式错误的工具，后来做了融合。

为了复用基于 estree 的一些 rule， @typescript-eslint/parser 把 ts node 转成了 estree node，但是依然保留了映射关系和一些操作 ts ast 的 api。

这样基于 estree AST 的 rule 可以正常运行，基于 ts AST 的 rule 也可以映射回原来的 ts node 然后运行。

通过这种方式，完美的把 eslint 和 tslint 融合在了一起。
          