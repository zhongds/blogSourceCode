---
title: Markdown常用命令及其效果
date: 2017-03-05 17:58:17
tags: Markdown 语法
---
<!--注释-->

### 标题
用#的个数来表示  
```
### 标题 ==> <h3>标题</h3>
```

### 注释
用html格式注释
<!--这是注释-->

### 分割线
___
---
***
```
//三种写法
___
---
***
```

### 换行
```
空两格 + 回车
```

### 段落缩进
```
输入法全角 + 空格
```

### 强调
**这是加粗**  
```
**这是加粗**  
```
*这是斜体*  
```
*这是斜体*  
```
~~这是删除~~  
```
~~这是删除~~  
```

### 列表  
+ 无序1
+ 无序2
- 无序3
* 无序4
```
// + - * 都表示无序
+ 无序1
+ 无序2
- 无序3
* 无序4
```
1. 有序1
2. 有序2
```
// 用数字表示
1. 有序1
2. 有序2
```

### checkbox
- [ ] 天才
- [ ] 人才
- [x] 勾选
```
- [ ] 天才
- [ ] 人才
- [x] 勾选
```

### 链接
[百度](https://www.baidu.com/)
```
[百度](https://www.baidu.com/)
```

[百度][1]不到, [google][2]一下

[1]: https://www.baidu.com/
[2]: https://www.google.com/
```
[百度][1]不到, [google][2]一下

[1]: https://www.baidu.com/
[2]: https://www.google.com/
```

### 图片
![image](http://note.youdao.com/favicon.ico "有道笔记")
```
![image](http://note.youdao.com/favicon.ico "有道笔记")
```

### 表格
header 1 | header 2 |
------   | ----- |
row 1 col 1 | row 1 col 2 |
row 2 col 1 | row 2 col 2 |

### highlight
```js
// ```js   （```` + language => highlight）
grunt.initConfig({
  assemble: {
    options: {
      assets: 'docs/assets',
      data: 'src/data/*.{json,yml}',
      helpers: 'src/custom-helpers.js',
      partials: ['src/partials/**/*.{hbs,md}']
    },
    pages: {
      options: {
        layout: 'default.hbs'
      },
      files: {
        './': ['src/templates/pages/index.hbs']
      }
    }
  }
};
```