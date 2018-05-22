### webpack3 与 webpack4 区别与注意事项

> 本文主要记录3版本、4版本之间的区别，同时在开发中要注意的事项；关于要注意的事项，只是记录自己遇到的差异项；

> 先学习个单词 bundle [ˈbʌndl] n.'捆;一批（同类事物或出售的货品）'

#### webpack4 对webpack进行了拆分

> webpack3中，webpack库中集结了webpack-cli，因此只需要安装webpack就能当做可执行文件使用；

> 在webpack4中，将原来的webpak库拆分成：webpack，webpack-cli；这两个都要安装；当我们要执行配置文件时，脚本如下：

```
./node_modules/.bin/webpack-cli --open --config webpack.config.js
```


#### output 输出的bundle.js结构发生变化

> 通常情况下，webpack会将所有模块(js/jsx/css/less/sass/json等)合并到bundle.js中；先来看下bundle.js的结构：

```
// webpack3 简写为如下结构

(function(modules){
	// 此函数相当于require，用于调用模块
	function __webpack_require__(moduleId){

		var module = installedModules[moduleId] = {
			i: moduleId,
			l: false,
			exports: {}
		};

		// Execute the module function
		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

	}

	return __webpack_require__(__webpack_require__.s = 32);

})([
	(function(){}), // 模块1
	(function(){}), // 模块2
	(function(){}), // 模块3
	(function(){}), // 模块4
	(function(){}), // 模块5
])
```
> 其实bundle.js结构就是个立即执行函数，参数为递归出的各个模块；当要使用某个模块时，就用__webpack_require__调用对应的模块序号：

```
if (process.env.NODE_ENV === 'production') {
  module.exports = __webpack_require__(33);
} else {
  module.exports = __webpack_require__(34);
}
```

> 从上述结构中，可以很清楚看到bundle.js是如何工作的：在立即执行函数执行后，首先执行的是序号为 32 的模块(此模块就是在配置文件中指定的入口文件)，在 序号32的模块中在继续调用别的模块；

> 从这种结构中，也很好理解代码文件的分割、合并；

> 那么 webpack4 的bundle.js有什么不同呢？看如下代码：

```
// webpack4 bundle.js 结构简写  [ mode = 'development' ]

(function(modules){
	// 此函数相当于require，用于调用模块
	function __webpack_require__(moduleId){

		var module = installedModules[moduleId] = {
			i: moduleId,
			l: false,
			exports: {}
		};

		// Execute the module function
		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

	}

	return __webpack_require__(__webpack_require__.s = "./app/app.js");

})({
	"./app/app.js": (function(){}), // 模块1
	"./app/components/app-header.js": (function(){}), // 模块2
	"./app/components/app-main.jsx": (function(){}), // 模块3
	"./app/components/app-nav.js": (function(){}), // 模块4
	"./app/icons/search.png": (function(){}), // 模块5
})
```

> 可以看到主体结构没有变化，只是立即执行函数的参数由 数组==>对象，而且对象中每个模块的key就是模块在文件中的相对路径；同时，模块中的代码都使用eval函数执行，也就是原先的代码都压缩成了字符串；

> webpack4中，bundle.js采用key-value寻找模块的方式更易读，但在模块对象的函数体中使用eval()函数，可能会影响效率；

> 以上看到的这些结构是在 mode='development' 下的；在 mode='production' 下代码完全压缩，没有使用eval；


#### webpack4 新增mode

> mode 就是设置当前配置为哪种环境；有三个值：

1. none

2. development // 生产环境

3. production // 产品环境


#### webpack4 压缩插件等发生改变


#### webpack4 无法使用插件extract-text-webpack-plugin

> 插件extract-text-webpack-plugin是用来将css文件从bundle.js中独立出来的；但是在webpack4中无法使用；
> 在下载安装此插件时，也会提示需要匹配webpack3

```
// webpack3 压缩css 提取css
	module:{
		rules:[{
				test: /(\.jsx|\.js)$/,
	            use: ["babel-loader?presets[]=env,presets[]=react"],
	            exclude: /node_modules/
			},{
				test: /\.css$/,

				// 通过参数 minimize 压缩css
				use: ExtractTextPlugin.extract({
		          fallback: "style-loader",
		          use: ["css-loader?minimize"]
		        }),
				exclude: /node_modules/
			}
		]
	},

	// 配置需要的plugins
	plugins:[
		// 生成入口文件HTML
		new HtmlWebpackPlugin({
            template: app_path + "/index.html"
        }),
		// 提取css文件
        new ExtractTextPlugin("styles.css"),
	]
```

























































































