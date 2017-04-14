# webpack学习笔记 （上）

## 前言

这几天晚上在跟着[阮一峰的Webpack教程](https://github.com/ruanyf/webpack-demos)复习Webpack，发现Webpack确实是一个非常优秀的前端工具，能做的事真是超级多。当然能做的事情越多，他的配置文件可能就越复杂。像我记性这么差的人。还是写个博客记录一下吧！毕竟好记性不如烂笔头！

## 关于webpack

webpack的配置文件是 `webpack.config.js` ， `webpack.config.js` 通过 `module.exports` 输出配置文件。

有了 `webpack.config.js` 之后，我们就可以通过在命令行执行如下命令使用webpack打包我们的代码了。

- `$ webpack` => 基本操作，为开发环境打包代码
- `$ webpack -p` => 为生产环境打包代码（压缩代码）
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

### 加载器（loader）

加载器是需要用webpack打包的资源文件的预处理器。我在这里将介绍 `Bable-loader` 、 `CSS-loader` 和 `Image-loader` 这三个加载器。

加载器需要在配置文件中的 `module.loaders`中定义，如下示例：

```
module.explorts = {
    // ..
    module: {
        loaders: {
            test: /\.js[x]?$/,  // 正则匹配需要用加载器预处理的文件
            exclude: /node_modules/,  // 正则匹配排除的文件或文件夹, 忽略则匹配所有涉及的文件
            include: /src/,  // 正则匹配包含的文件或文件，忽略则匹配所有涉及的文件
            loader: 'babel-loader'  // 加载器
        }
    }
}
```

#### bable-loader

`Bable-loader` 可以将 JSX/ES6 文件转化为 ES5 规范下的 JS 文件。如下示例：

```
module.exports = {
  // ...
  module: {
    loaders:[
      {
        test: /\.js[x]?$/,
        exclude: /node_modules/,
        loader: 'babel-loader?presets[]=es2015'
      },
    ]
  }
};
```

这里 `babel-loader` 需要用到 `babel-preset-es2015` 插件，将 `ES2015` 语法转化为 `ES5` 语法，如上示例是一种写法，还有另一种写法，如下所示：

```
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: 'babel',
      query: {
        presets: ['es2015']
      }
    }
  ]
}
```

#### css-loader

如果我们在被 Webpack 处理的文件中引入了 CSS 文件，我们就需要使用 `CSS-loader` 来处理这些 CSS 文件，如下示例：

```
module.exports = {
  // ..
  module: {
    loaders:[
      { 
          test: /\.css$/, 
          loader: 'style-loader!css-loader' },
    ]
  }
};
```

注意，我们这里使用两个加载器来转换 CSS 文件， 一个是 `css-loader` 来编译CSS文件， 另一个是 `style-loader` 将 CSS 代码引入 HTML 文件中。

两个不同的加载器需要用 `!` 来连接。

#### image-loader

Webpack 也可以打包图片到JS中，如下示例：

```
module.exports = {
  // ...
  module: {
    loaders:[
      { 
          test: /\.(png|jpg)$/, 
          loader: 'url-loader?limit=8192' }
    ]
  }
};
```

看这里 `url-loader?limit=8192` ，我们之前在 `bable-loader` 里见过， 这次的查询关键词是 `limit` ，意义是最大限制，即 如果小于 8192B 的图片文件将会被转换，小于8192B 的图片文件将不会被转换。

### 插件（plugin）

Webpack有一套插件系统来扩展他的功能，我们之前说了加载器是在配置文件中的 `modules.loaders` 中声明，而插件就是在 `plugins` 中声明。

我们在这里介绍三个插件 `uglifyJsPlugin` 、 `html-webpack-plugin` 和 `open-browser-webpack-plugin` 。

如它的名字， `uglifyJsPlugin` 作用是压缩JS代码，是 Webpack 自带的插件。

`html-webpack-plugin` 和 `open-browser-webpack-plugin` 都是 Webpack 的第三方插件。 `html-webpack-plugin` 的作用是自动生成一个 HTML 文件。 `open-browser-webpack-plugin` 的作用是可以在 Webpack 加载时自动打开一个浏览器页面，这个可就厉害了，配合 `webpack-dev-server` 我们可以在 Webpack 执行后，不用动手就看到效果。

如下示例展示了这三个插件的声明方法。

```
var webpack = require('webpack');
var uglifyJsPlugin = webpack.optimize.UglifyJsPlugin;
module.exports = {
  // ..
  plugins: [
    new uglifyJsPlugin({
      compress: {
        warnings: false
      }
    }),
    new HtmlwebpackPlugin({
      title: 'Webpack-demos',
      filename: 'index.html'
    }),
    new OpenBrowserPlugin({
      url: 'http://localhost:8080'
    })
  ]
};
```

这样，我们不需要写 HTML 文件，运行 `webpack-dev-server` ， Webpack就自动为我们打包代码、压缩代码、创建 HTML 文件、打开浏览器。帅！

### 环境标识

Webpack 允许我们使用环境标识来区分开发环境和生产环境，从而让我们在开发环境中执行一些额外的代码。

比如，如果我们只想在开发环境中执行 `document.write(new Date())` 我们可以这样写我们的入口文件

```
document.write('<h1>Hello World</h1>');

if (devEnvironment) {
  document.write(new Date());
}
```

然后在配置文件中使用 `DefinePlugin` 插件：

```
var webpack = require('webpack');

var devFlagPlugin = new webpack.DefinePlugin({
  devEnvironment: JSON.stringify(JSON.parse(process.env.DEBUG || 'false'))
});

module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  },
  plugins: [devFlagPlugin]
};
```

最后，我们只需要在执行 Webpack 命令时传入环境标识，Webpack就会为我们将环境标识设为我们需要的值，从而确定执不执行一些代码。

```
$ env DEBUG=true webpack  // 此时为开发环境，执行额外代码
$ webpack  // 此时为生产环境，不执行额外代码
```

(未完待续)