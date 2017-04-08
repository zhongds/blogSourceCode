---
title: eslint + pre-commit检测代码
date: 2017-04-06 21:31:56
tags: eslint、pre-commit、npm script
---
　　良好的代码规范有助于项目的维护和新人的快速上手。前段时间，把eslint引入了项目中做静态代码检查。 一下把所有的代码都改造是不可能，要改的地方太多，而且要保证后来提交代码的质量。于是有了eslint + pre-commit 的结合。
　　pre-commit是git的钩子，顾名思义就是在提交前运行，所以一般用于代码检查、单元测试。git还有其他钩子，比如prepare-commit-msg、pre-push等，具体可查看[git官网](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)。git 钩子目录在.git/hooks下（如下图）：
![git-hooks.png](http://upload-images.jianshu.io/upload_images/4928078-3e45a2c94dc9fcd8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　上图这些文件都是对应钩子的示例脚本，.sample后缀都是出于未启动状态。对应的钩子要生效，把.sample去掉。示例都是用shell脚本写的。那如果想用js写怎么办呢？需要借助[pre-commit库](https://github.com/observing/pre-commit)：
1. 安装pre-commit
```
npm install pre-commit --save-dev
```
2. 配置package.json
 - 执行静态文件检查
  ```
  // package.json
  "scripts": {
      "lint": "eslint src --ext .js --cache --fix",
    },
    "pre-commit": [
      "lint",
    ]
  ```
上面配置会保证eslint在提交时会校验src目录下的js文件。
那如果要动态获取提交的代码进行校验呢？
 - 校验提交代码
  ```
  // 获取修改后的js文件
  git diff HEAD --name-only| grep .js$
  ```
  package.json文件可改为：
  ```
  // package.json
"scripts": {
       "lint": "eslint src --ext .js --cache --fix",
       "pre-lint": "node check.js"
  },
  "pre-commit": [
       "pre-lint",
  ]
  ```
在check.js中需要调用eslint的Node.js API，详情可看[eslint官网](http://eslint.org/docs/developer-guide/nodejs-api)。以下是我在项目中的例子，可作为参考：
```
// check.js
const exec = require('child_process').exec
const CLIEngine = require('eslint').CLIEngine
const cli = new CLIEngine({})
function getErrorLevel(number) {
       switch (number) {
          case 2:
            return 'error'
          case 1:
            return 'warn'
         default:
       }
       return 'undefined'
}
let pass = 0
exec('git diff --cached --name-only| grep .js$', (error, stdout) => {
    if (stdout.length) {
        const array = stdout.split('\n')
        array.pop()
        const results = cli.executeOnFiles(array).results
        let errorCount = 0
        let warningCount = 0
        results.forEach((result) => {
            errorCount += result.errorCount
            warningCount += result.warningCount
            if (result.messages.length > 0) {
                console.log('\n')
                console.log(result.filePath)
                result.messages.forEach((obj) => {
                    const level = getErrorLevel(obj.severity)
                    console.log(`   ${obj.line}:${obj.column}  ${level}  ${obj.message}  ${obj.ruleId}`)
                    pass = 1
                })
            }
        })
        if (warningCount > 0 || errorCount > 0) {
            console.log(`\n   ${errorCount + warningCount} problems (${errorCount} ${'errors'} ${warningCount} warnings)`)
        }
        process.exit(pass)
    }
    if (error !== null) {
        console.log(`exec error: ${error}`)
    }
})
```