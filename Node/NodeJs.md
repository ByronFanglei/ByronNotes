# NodeJs
## 安装
>[官网下载](https://nodejs.org/zh-cn/)
>nvm安装,需要先安装nvm，可以切换不同的node版本
>>mac直接去[Homebrew](https://brew.sh/)官网安装
>>windows直接去GitHub下载[nvm](https://github.com/coreybutler/nvm-windows)然后运行 nvm install v版本号 安装对应的nodejs版本

## ECMAScript,nodejs与JavaScript区别
* ECMAScript
>定义语法，写JavaScript与nodejs都必须遵守
>变量定义，循环，判断，函数
>原型链和原型，作用域和闭包，异步
* JavaScript
>使用ECMAScript语法规范，外加Web API
>DOM操作，BOM操作 事件绑定，Ajax
* nodejs
>使用ECMAScript语法规范，外加nodejs API
>处理http，处理文件等

## 博客项目开发
### 接口开发(原生Nodejs)
##### nodejs处理http请求
* 从输入URL到打开页面经历了什么？---面试问题
>DNS解析，建立TCP连接，发送http请求
>sever接收到http请求，处理，并返回
>客户端接收返回数据，处理数据（渲染页面，执行js）

* GET
```JavaScript
const http = require('http')
const querystring = require('querystring')
const sever = http.createServer((req, res) => {
  console.log('method:', req.method)
  const url = req.url
  console.log('url:', url)
  req.query = querystring.parse(url.split('?')[1])
  const path = url.split('?')[0]
  console.log('query:',req.query)
  console.log('path',path)
  res.end(
    JSON.stringify(req.query)
  )
})
sever.listen(8000)
console.log('ok')
```
* POST
```JavaScript
const http = require('http')
const querystring = require('querystring')
const sever = http.createServer((req, res) => {
  if(req.method==='POST'){
    // 数据格式
    console.log('content-type:',req.headers['content-type'])
    // 接收数据
    let postData = ''
    req.on('data', chunk => {
      postData+=chunk.toString()
    })
    req.on('end', () => {
      console.log('postData:',postData)
      res.end('hello world')
    })
  }
})
sever.listen(8000)
console.log('ok')
```
##### 搭建开发环境
* nodemon监测文件变化
* cross-env设置环境变量

##### 开发接口

















