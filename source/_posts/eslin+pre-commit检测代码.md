---
title: eslint + pre-commit检测代码
date: 2017-04-06 21:31:56
tags: eslint、pre-commit、npm script
---
　　良好的代码规范有助于项目的维护和新人的上手。前段时间把eslint引入了项目中做静态代码检查。一下把所有的代码都改造是不可能的，要改的地方太多了，而且还要保证后来提交的质量。于是有了eslint + pre-commit 的结合。
　　pre-commit是git的钩子，顾名思义就是在提交前执行，所以一般用于代码检查、单元测试。git还有其他钩子，比如prepare-commit-msg、pre-push等，具体可查看[git官网](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks).git钩子目录在.git/hooks下(如下图): 
{% asset_img git-hooks.png git hooks 目录 %}

