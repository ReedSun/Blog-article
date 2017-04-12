# webpack学习笔记

## 前言

这几天晚上在跟着[阮一峰的Webpack教程](https://github.com/ruanyf/webpack-demos)复习Webpack，发现Webpack确实是一个非常优秀的前端工具，能做的事真是超级多。当然能做的事情越多，他的配置文件可能就越复杂。像我记性这么差的人。还是写个博客记录一下吧！毕竟好记性不如烂笔头！

## 关于webpack

webpack的配置文件是 `webpack.config.js` ， `webpack.config.js` 通过 `module.exports` 输出配置文件。

有了 `webpack.config.js` 之后，我们就可以通过在命令行执行如下命令使用webpack打包我们的代码了。

- `$ webpack` => 基本操作，为开发环境打包代码
- `$ webpack -p` => 为生产环境打包代码
- `$ webpack --watch` => 增量打包代码 （每次修改文件即会自动重新打包文件）
- `$ webpack -d` => 打包同时生成source maps位置文件
- `$ webpack --colors` => 让打包信息使用不同的颜色，易于观察

我们可以使用 `npm` 配置文件 `package.json` 的 `scripts` 命令简化我们的操作，示例如下所示：
```
// package.json
{
    // ...
    "script": {
        "dev" : "webpack --watch --colors"
    },
    // ...
}
```

## 关于webpack Dev Server

关于 `webpack Dev Server` ，[webpack的GitHub](https://github.com/webpack/webpack-dev-server)是这么介绍它的：
> Serves a webpack app. Updates the browser on changes.

`webpack Dev Server`是一个小型的服务器，为我们的webpack应用服务。与 `webpack` 类似，我们在命令行中通过执行如下指令打开服务器、打包代码。
```
$ webpack-dev-server
```

我们打开了服务器，就可以在浏览器中通过`http://loaclhost:8080`访问网页，查看我们打包的代码。

同时， `webpack-dev-server` 还支持热重载，即我们更新了我们代码，页面将自动刷新并打包我们的代码。

## 正式开始

### 入口文件


入口文件是 `webpack` 运行并打包文件的入口文件。Webpack 会分析入口文件，解析包含依赖关系的各个文件。将这些文件（模块）都打包到 bundle.js 。我们通过 `entry` 属性来指定它。如下示例：
```
// webpack.config.js
module.exports = {
    entry: './main.js',
    output: {
        filename: 'bundles.js'
    }
}
```
此时入口文件是 `main.js`， `webpack` 会将 `main.js` 打包到 `bundles.js` 文件中。

入口文件也可以是多个文件，这对一个多页应用来说很有帮助。如下示例：
```
module.exports = {
    entry: {
        bundle1: './main1.js',
        bundle2: './main2.js'
    },
    output: '[name].js]'
}
```
此时入口文件有两个，分别是 `main1.js` 和 `main2.js` ， `webpack` 会将这两个文件分别打包到 `bundle1.js` 和 `bundle2.js` 中。