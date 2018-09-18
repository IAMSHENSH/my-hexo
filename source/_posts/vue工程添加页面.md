---
title: VUE 工程添加页面
date: 2018-06-07
tags: 
- vue
- front
- webpack
categories: Front-end

---

> 关于在 vue + webpack 工程内添加页面的步骤记录
> vue-cli 2.*

<!-- more -->

以 test 页面为例子

# 添加文件

- 「test.html」 根目录下添加

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>DEMO</title>
</head>
<body>
<div id="test"></div>
</body>
</html>
```

- 「test.js」 -> 'src/pages/test/test.js'

```js
import Vue from 'vue'
import test from './test.vue'
Vue.config.productionTip = false;
new Vue({
  el: '#test',
  render: h => h(test)
})
```

- 「test.vue」 -> 'src/pages/test/test.js'

```html
<template>
  <div id="test"></div>
</template>
<script>
  export default {
    name:'test',
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
</style>
```

# 添加配置

1. 在 'config/index.js' 内添加第4行代码:

```js
  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),
    test:path.resolve(__dirname, '../dist/test.html'),
    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
```

1. 在 'build/webpack.base.conf.js' 内添加第5行代码

```js
module.exports = {
  context: path.resolve(__dirname, '../'),
  entry: {
    app: './src/main.js',
    test:'./src/pages/test/test.js',
  },
```

1. 在 'build/webpack.dev.conf.js' 的 `plugins:[]` 内添加

```js
    new HtmlWebpackPlugin({
      filename: 'test.html',
      template: 'test.html',
      inject: true,
      chunks:['test']     
    }),
```

1. 在 'build/webpack.prod.conf.js' 的 `plugins:[]` 内添加

```js
    new HtmlWebpackPlugin({
      filename: config.build.test,
      template: 'test.html',
      inject: true,
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
      },
      chunksSortMode: 'dependency',
      chunks:['manifest','vendor','test']
    }),
```

# 完成：重新运行
