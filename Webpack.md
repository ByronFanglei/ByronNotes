# WebPack
* WebPack是什么：模块打包工具，可以识别任何模块引入的语法
## 安装WebPack
* 安装
```shell
# 一般在项目中安装，而不是全局安装
npm install webpack webpack-cli
#查看版本
npx webpack -v
```

## 引入规范
### ES moudule 模块引入规范
```javascript
// 引入
import Header from './header'
// 导出
export default Header
```

### CommonJS 模块引入规范
```javascript
// 引入
const Header = require('./header.js')
// 导出
module.exports = Header
```

### CMD
### AMD

## Webpack配置
### 基础配置
* 配置文件为`webpack.config.js`
```javascript
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  // 打包文件压缩
  // mode: 'production',
  // 打包文件不压缩
  mode: "development",
  // 项目入口文件
  entry: './src/index.js',
  // 针对不同类型文件进行打包
  module: {
    rules:[{
      // 当打包后缀为jpg的文件时，使用file-loader进行打包
      test: /\.(jpg|png|gif)$/,
      use: {
        loader: 'url-loader',
        options: {
          // 打包后的名字配置
          name: '[name]_[hash].[ext]',
          // 打包后存入文件地址
          outputPath: 'images/',
          // 当图片大于2048后不转为base64文件，存入images，反之转为base64文件
          limit: 2048
        }
      }
    }, {
      test: /\.scss$/,
      use: [
        // 将css资源挂载到header中
        'style-loader', 
        {
          // 解析css
          loader: 'css-loader',
          options: {
            // 当使用import引入的scss文件后，先调用下面两个loader，再向上调用，否则直接向上调用
            importLoaders: 2,
            // css模块化打包，css只在当前文件生效
            // modules: true,
          }
        }, 
        // 解析scss
        'sass-loader', 
        // 样式资源浏览器前缀
        'postcss-loader'
      ]
    }, {
      // 字体打包
      test: /\.(eot|ttf|svg|woff)$/,
      use: {
        loader: 'file-loader'
      }
    }]
  },
  plugins: [
    // 打包自动生成一个入口文件，以index.html为模板生成
    new HtmlWebpackPlugin({
      template: 'index.html'
    }),
    // 打包前自动删除dist目录下的文件
    new CleanWebpackPlugin()
  ],
  // 项目输出文件，最后要运行的文件
  output: {
    // 在index.html文件引入js文件添加前缀
    publicPath: 'http://cdn.fbyron.cn',
    // 打包最后生成的文件
    filename: 'main.js',
    // 打包最后生成的文件夹
    path: path.resolve(__dirname, 'dist')
  }
}
```

### loader是什么
* webpack不能识别非js后缀的文件，所以需要使用loader让webpack识别出来
* loader的执行顺序为：从下到上，从右到左

#### 常用的loader
* 静态资源打包`file-loader` ：打包静态资源eg：png、jpg、gif、txt等
* 静态资源打包`url-loader` ： 功能类似于`file-loader`，可以将图片打包成base64地址，一般用于小图片
* 样式资源打包`css-loader`，`style-loader`：将css资源挂载到header中
* 预加载样式资源打包`sass-loader`：打包scss文件，less，styles文件同理
* 样式资源浏览器前缀`postcss-loader`：

```javascript
// 第一步 安装postcss-loader,autoprefixer
// 第二步 webpack.config.js进行配置
{
  test: /\.css$/,
  use: [
    'style-loader', 
    'css-loader',  
    'postcss-loader'
  ]
}
// 第三步 在webpack.config.js文件同级目录中，创建postcss.config.js并配置
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}
// 第四步 package.json 进行浏览器支持配置（与 devDependencies 同级）
"browserslist": [
    "defaults",
    "not ie <= 8",
    "last 2 versions",
    "> 1%",
    "iOS >= 7",
    "Android >= 4.0"
  ],
// 第五步 打包即可
```

### Plugins是什么
* 可以再webpack运行到某个时刻的时候，帮你做的一些事情

#### 常用的Plugins
* `html-webpack-plugin`：打包结束后自动生成一个html文件，并把打包生成的js自动引入到这个html文件中
* `clean-webpack-plugin`：自动删除未使用的文件，一般为dist文件

### Entry与Output
* entry：项目入口文件

```javascript
// 打包生成两个文件
entry: {
    main: './src/index.js',
    sub: './src/index.js',
  },
output: {
    // 打包最后生成的文件，这里需要使用[name]作为占位符
    filename: '[name].js',
    // 打包最后生成的文件夹
    path: path.resolve(__dirname, 'dist')
  }
// 打包结束后，会在dist文件生成main.js与sub.js，同时index.html会自动引入这两个js文件
```

* output：项目输出文件，最后要运行的文件

```javascript
// 项目输出文件，最后要运行的文件
  output: {
    // 在index.html文件引入js文件添加前缀
    publicPath: 'http://cdn.fbyron.cn',
    // 打包最后生成的文件
    filename: 'bundle.js',
    // 打包最后生成的文件夹
    path: path.resolve(__dirname, 'dist')
  }
// index.html
<script src="http://cdn.fbyron.cn/bundle.js"></script>
```

### SourceMap
* SourceMap是什么：举个例子，当原始文件中有js错误，打包完成后，网页报错，查看报错信息只能看到对应打包后的文件中的错误，而看不到原始文件中是哪里的错误，SourceMap是一个映射关系，他能够映射到原始文件的行数，更好的排错

```javascript
// 生产环境一般使用cheap-module-source-map进行打包
mode: 'production',
devtool: "cheap-module-source-map",

// 开发环境一般使用eval-cheap-module-source-map进行打包
mode: "development",
devtool: "eval-cheap-module-source-map",
```

### WebpackDevServer



























