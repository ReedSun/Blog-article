# webpack 学习笔记（下）

### 代码分割

对于一个大型的 Webapp 来说，把所有 JS 文件都放在同一个文件中是肯定不合理的！这会大大影响它的性能。webpack 允许我们把代码分割成几块。如果一些代码仅会在一些特定的情况下才执行时，webpack 会按需加载他们。

#### require.ensure

我们可以使用 `require.ensure` 来定义一个分割点来实现代码分割。如下实例

```
// main.js
document.write('I will perform right now')
setTimeout(function(){
	require.ensure(['./a'], function(require) {
	  var content = require('./a');
	  document.open();
	  document.write(content);
	  document.close();
	});
}, 3000)
```

```
// a.js
module.exports = 'I will perform in 3 seconds.'
```

```
// webpack.config.js
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
}
```

`webpack.config.js` 不需要做任何特殊处理，我们会发现 webpack 打包生成了两个文件 `bundle.js` 和 `0.bundle.js` ，打开网页我们会发现3秒之后 `0.bundle.js` 才会加载。

#### bundle-loader

除了用上面的 `require.ensure`，我们还可以用 `bundle-loader` 来实现代码分割。实例与 `require.ensure` 类似，只有 `main.js` 文件略有不同。

```
document.write('I will perform right now')
setTimeout(function(){
	var load = require('bundle-loader!./a.js')
	load(function(content) {
	  document.open();
	  document.write(content);
	  document.close();
	});
}, 3000)
```
webpack 同样打包生成了两个文件 `bundle.js` 和 `0.bundle.js` ，同时也实现了按需加载。

####　通用的代码块

我们先来看一个例子：

```
// main1.js
import Vue from 'vue'
var vm = new Vue({
	el: '#a',
	data: {
		a: "Hello"
	}
})
```

```
// main2.js
import Vue from 'vue'
var vm = new Vue({
	el: '#b',
	data: {
		b: "ReedSun"
	}
})
```

```
// index.html
<html>
	<body>
		<div id="a">{{a}}</div>
		<div id="b">{{b}}</div>
		<script src="bundle1.js"></script>
		<script src="bundle2.js"></script>
	</body>
</html>
```

```
// webpack.config.js
module.exports = {
	entry: {
		bundle1: './main1.js',
		bundle2: './main2.js'
	},
	output: {
		filename: '[name].js'
	},
	resolve: {
		alias: {
			'vue': 'vue/dist/vue.js'
		}
	}
}
```

`webpack.config.js`中的 `resolve` 是别名的意思，通过配置它，我们可以直接使用 `require('vue')`， 而不用使用 `require('vue/dist/vue.js')` 了。

这是一个用Vue渲染页面的简单用例，我们使用 webpack 打包后惊人的发现， webpack在两个脚本中将 `Vue` 打包了两次！ 这怎么能行！还好，万能的 webpack 提供了 `CommonsChunkPlugin` 插件，这个插件可以将不同脚本中引用的相同的代码块提取出来，打包成一个单独通用代码块，厉害了我的哥！

使用 `CommonsChunkPlugin` 插件，我们需要对 `index.html` 和 `webpack.config.js` 做如下修改：

```
// index.html
<html>
	<body>
		<div id="a">{{a}}</div>
		<div id="b">{{b}}</div>
		<script src="common.js"></script>
		<script src="bundle1.js"></script>
		<script src="bundle2.js"></script>
	</body>
</html>
```

```
var CommonsChunkPlugin = require("webpack/lib/optimize/CommonsChunkPlugin")
module.exports = {
	entry: {
		bundle1: './main1.js',
		bundle2: './main2.js'
	},
	output: {
		filename: '[name].js'
	},
	resolve: {
		alias: {
			'vue': 'vue/dist/vue.js'
		}
	},
	plugins: [
		new CommonsChunkPlugin('common')
	]
}
```

嗯~这样一来，通用的依赖只会被 Webpack 打包一次，优化了好多性能。

#### Vendor chunk

上例中，我们用 `CommonsChunkPlugin` 将通用的代码块分离出来。当然，我们也可以用它来将第三方库类文件和我们自己写的代码分离。示例代码如下：

```
\\ index.html
<html>
	<body>
		<div id="a">{{a}}</div>
		<script src="vendor.js"></script>
		<script src="bundle.js"></script>
	</body>
</html>
```

```
\\ main.js
import Vue from 'vue'
var vm = new Vue({
	el: '#a',
	data: {
		a: "Hello"
	}
})
```

```
\\ webpack.config.js
var CommonsChunkPlugin = require("webpack/lib/optimize/CommonsChunkPlugin")
module.exports = {
	entry: {
		app: './main.js',
		vendor: ['vue']
	},
	output: {
		filename: 'bundle.js'
	},
	resolve: {
		alias: {
			'vue': 'vue/dist/vue.js'
		}
	},
	plugins: [
		new webpack.optimize.CommonsChunkPlugin(/* chunkName= */'vendor', /* filename= */'vendor.js')
	]
}
```

执行 `webpack` 之后，程序会把代码打包到两个文件 `bundls.js` 和 `vendor.js` ，这样就把我们自己的代码和我们引入的第三方库进行了分离。