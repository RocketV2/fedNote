### webpack 官方可视化分析工具

> 我们对输出结果需要进行分析，以决定下一步的优化方向；直接阅读bundle.js源码是不可取的，因为可读性太差；

> 还好官方提供了可视化的分析工具；

> 下面介绍如何使用可视化分析工具；

##### 生成stats.json文件

> 在启动webpack时，添加如下参数：

1. --profile: 记录构建过程中的耗时信息

2. --json: 以json的格式输出构建结果，该json文件包括了所有构建相关的信息；

> 启动webpack时，运行一下命令：

```
webpack --profile --json > stats.json
```

> 运行成功后，会在根目录下生成stats.json文件；


##### 将stats.json文件导入可视化工具中

> 官方可视化工具是一个web应用，网址为 [webpack可视化工具](http://webpack.github.io/analyse)

> 将json文件导入，就可以得到分析结果；


### 非官方可视化分析工具

> 另一种可视化分析工具 webpack-bundle-analyzer ，虽然没有 webpack analyse 功能多，但是更直观；

> 优点是可以让我们知道：

1. 打包出的文件包含什么

2. 每个文件的大小

3. 模块之间的关系

4. 每个文件Gzip后的大小

##### 如何使用 webpack-bundle-analyzer

> 1. 安装 webpack-bundle-analyzer 到全局，执行如下命令：

```
npm install -g webpack-bundle-analyzer
```

> 2. 生成的stats.json

> 3. 在项目的根目录执行 webpack-bundle-analyzer 后，浏览器会打开网页；

##### 在项目中安装 webpack-bundle-analyzer

> 有时我们只想在当前项目中使用 webpack-bundle-analyzer ,那么我们就可以在局部安装这个库了；

> 1. 局部安装

```
cnpm i -D webpack-bundle-analyzer
```

> 2. 生成stats.json

> 3. 在package.json中的scripts中配置，注意一定要填写 stats.json 的路径
```
./node_modules/.bin/webpack-bundle-analyzer ./stats.json
```

> 可以同时使用这两种分析工具，各有利弊，权衡使用；















