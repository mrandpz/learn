
- cnpm install --save-dev webpack 本地安装
- 启用

		"scripts": {
		    "start": "webpack --config webpack.config.js"
		}
- 加载css

	npm install --save-dev style-loader css-loader


		module:{
	        rules:[
	            {
	                test:'/.css$/',
	                use:[
	                    'style-loader',
	                    'css-loader'
	                ]
	            }
	        ]
	    }
- 加载图片

		npm install --save-dev file-loader

- 加载字体

		+       {
		+         test: /\.(woff|woff2|eot|ttf|otf)$/,
		+         use: [
		+           'file-loader'
		+         ]
		+       }

- HtmlWebpackPlugin 解决html中引入文件

- clean-webpack-plugin 清理构建前文件夹

			const CleanWebpackPlugin = require('clean-webpack-plugin');
		...
		
		 	plugins: [
		+     new CleanWebpackPlugin(['dist']),
		      new HtmlWebpackPlugin({
		        title: 'Output Management'
		      })
		    ],

- devtool: 'inline-source-map',  (生产环境使用source-map)
- webpack-dev-server 为你提供了一个简单的 web 服务器
	
	webpack.config.js

		+   devServer: {
		+     contentBase: './dist'
		+   },

- webpack-dev-middleware 需要配合webpack-hot-middleware实现热替换 是一个中间件容器(wrapper)，它将通过 webpack 处理后的文件发布到一个服务器(server)


有三种常用的代码分离方法：

- 入口起点：使用 entry 配置手动地分离代码。
- 防止重复：使用 CommonsChunkPlugin 去重和分离 chunk。
- 动态导入：通过模块的内联函数调用来分离代码

CommonsChunkPlugin 防止重复


	+   externals: {
	+     lodash: {
	+       commonjs: 'lodash',
	+       commonjs2: 'lodash',
	+       amd: 'lodash',
	+       root: '_'
	+     }
	+   }


prividePlugin 很好的配合了tree shaking


	plugins: [
      new webpack.ProvidePlugin({
	      _: 'lodash'
       	join: ['lodash', 'join']
      })
    ]

	element.innerHTML = join(['Hello', 'webpack'], ' ');



HMR


	const path = require("path")
	const express = require("express")
	const webpack = require("webpack")
	
	
	const webpackDevMiddleware = require("webpack-dev-middleware")
	const webpackHotMiddleware = require("webpack-Hot-middleware")
	const webpackConfig = require('./webpack.config.js')
	
	const app = express(),
	            DIST_DIR = path.join(__dirname, "dist"), // 设置静态访问文件路径
	            PORT = 9090, // 设置启动端口
	            complier = webpack(webpackConfig)
	
	
	let devMiddleware = webpackDevMiddleware(complier, {
	    publicPath: webpackConfig.output.publicPath,
	    quiet: true //向控制台显示任何内容 
	})
	
	let hotMiddleware = webpackHotMiddleware(complier,{
	   log: false,
	   heartbeat: 2000,
	})
	app.use(devMiddleware)
	
	app.use(hotMiddleware);
	
	
	// 这个方法和下边注释的方法作用一样，就是设置访问静态文件的路径
	app.use(express.static(DIST_DIR))
	
	// app.get("*", (req, res, next) =>{
	//     const filename = path.join(DIST_DIR, 'index.html');
	
	//     complier.outputFileSystem.readFile(filename, (err, result) =>{
	//         if(err){
	//             return(next(err))
	//         }
	//         res.set('content-type', 'text/html')
	//         res.send(result)
	//         res.end()
	//     })
	// })
	
	app.listen(PORT,function(){
	    console.log("成功启动：localhost:"+ PORT)
	})

webpack.config.js



	const path = require('path');
	const HtmlwebpackPlugin = require('html-webpack-plugin');
	const webpack = require('webpack');
	
	
	//定义了一些文件夹的路径
	const ROOT_PATH = path.resolve(__dirname);
	const APP_PATH = path.resolve(ROOT_PATH, 'app');
	const BUILD_PATH = path.resolve(ROOT_PATH, 'build');
	
	module.exports = {

实现刷新浏览器webpack-hot-middleware/client?noInfo=true&reload=true 是必填的

	    entry: ['webpack-hot-middleware/client?noInfo=true&reload=true' , APP_PATH],
	    //输出的文件名 合并以后的js会命名为bundle.js
	    output: {
	        path: BUILD_PATH,
	        filename: 'bundle.js'//将app文件夹中的两个js文件合并成build目录下的bundle.js文件
	    },
	    //添加我们的插件 会自动生成一个html文件
	    plugins: [
	        new HtmlwebpackPlugin({
	            title: 'Hello World app'
	        }),//在build目录下自动生成index.html，指定其title
	
	        // 实现刷新浏览器必写
	         new webpack.HotModuleReplacementPlugin(),
	    ],
	
	};

	


