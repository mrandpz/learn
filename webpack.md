#webpack

##四个概念

入口（entry）
- 
	const config = {
	  entry: {
	    app: './src/app.js',
	    vendors: './src/vendors.js'
	  }
	};
	

输出（output）
-
如果配置创建了多个单独的 "chunk"（例如，使用多个入口起点或使用像 CommonsChunkPlugin 这样的插件），则应该使用占位符(substitutions)来确保每个文件具有唯一的名称。

	{
	  entry: {
	    app: './src/app.js',
	    search: './src/search.js'
	  },
	  output: {
	    filename: '[name].js',
	    path: __dirname + '/dist'
	  }
	}

使用 CDN 和资源 hash ？ 如何上传到cdn

	output: {
	  path: "/home/proj/cdn/assets/[hash]",
	  publicPath: "http://cdn.example.com/assets/[hash]/"
	}

在编译时不知道最终输出文件的 publicPath 的情况下，publicPath 可以留空，并且在入口起点文件运行时动态设置。如果你在编译时不知道 publicPath，你可以先忽略它，并且在入口起点设置 “__webpack_public_path__”。

loader   
-
让 webpack 能够去处理那些非 JavaScript 文件
现在，当该模块运行时，含有 CSS 字符串的 style 标签，将被插入到 html 文件的 <head> 中。

	module: {
	    rules: [
	      {
	        test: /\.css$/,
	        use: [
	          { loader: 'style-loader' },
	          {
	            loader: 'css-loader',
	            options: {
	              modules: true
	            }
	          }
	        ]
	      }
	    ]
	  }
阿

插件（plugins）   
-
插件目的在于解决 loader 无法实现的其他事。

使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。

	const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过 npm 安装
	const webpack = require('webpack'); //访问内置的插件
	const path = require('path');
	
	const config = {
	  entry: './path/to/my/entry/file.js',
	  output: {
	    filename: 'my-first-webpack.bundle.js',
	    path: path.resolve(__dirname, 'dist')
	  },
	  module: {
	    loaders: [
	      {
	        test: /\.(js|jsx)$/,
	        use: 'babel-loader'
	      }
	    ]
	  },
	  plugins: [
	    new webpack.optimize.UglifyJsPlugin(),
	    new HtmlWebpackPlugin({template: './src/index.html'})
	  ]
	};



使用 webpack-dev-server webpack-dev-server 为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)。让我们设置以下：

	+   devServer: {
	+     contentBase: './dist'
	+   },

	"start": "webpack-dev-server --open",

webpack-dev-middleware

	  const path = require('path');
	  const HtmlWebpackPlugin = require('html-webpack-plugin');
	  const CleanWebpackPlugin = require('clean-webpack-plugin');
	
	  module.exports = {
	    entry: {
	      app: './src/index.js',
	      print: './src/print.js'
	    },
	    devtool: 'inline-source-map',
	    plugins: [
	      new CleanWebpackPlugin(['dist']),
	      new HtmlWebpackPlugin({
	        title: 'Output Management'
	      })
	    ],
	    output: {
	      filename: '[name].bundle.js',
	      path: path.resolve(__dirname, 'dist'),
	+     publicPath: '/'
	    }
	  };

配置文件默认为 webpack.config.js，如果你想使用其它配置文件，可以加入这个参数。

	webpack --config example.config.js

环境选项

	webpack --env.production    # 设置 env.production == true
	webpack --env.platform=web  # 设置 env.platform == "web"

此插件可以通过合并的方式，后处理你的 chunk，以减少请求数。

	new webpack.optimize.LimitChunkCountPlugin({
	  maxChunks: 5, // 必须大于或等于 1
	  minChunkSize: 1000 // 设置 chunk 的最小大小。
	})