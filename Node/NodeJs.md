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

## 要点
### nodejs处理http请求
* 从输入URL到打开页面经历了什么？---面试问题
>DNS解析，建立TCP连接，发送http请求
>sever接收到http请求，处理，并返回
>客户端接收返回数据，处理数据（渲染页面，执行js）

* GET
```JavaScript
const http = require('http')
const querystring = require('querystring')

const server = http.createServer((request, respond) => {
  console.log(`method: ${request.method}`) // GET
  const url = request.url // 获取请求的完整url
  console.log(`url: ${url}`)
  request.query = querystring.parse(url.split('?')[1]) // 解析querystring
  console.log('query:',request.query)
  const path = url.split('?')[0]
  console.log('path',path)
  respond.end(
    JSON.stringify(request.query)
  )
})

server.listen(5000)
console.log('ok')
```
* POST
```JavaScript
const http = require('http')

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

```javascript
// get,post结合示例
const http = require('http')
const querystring = require('querystring')

const serve = http.createServer((request, respond) => {
  // 请求方式
  const method = request.method
  // 请求url
  const url = request.url
  // 请求path
  const path = url.split('?')[0]
  // GET接收数据
  const query = querystring.parse(url.split('?')[1])

  // 设置返回格式
  respond.setHeader('Content-type', 'application/json')
  // 返回数据
  const resData = {
    method,
    url,
    path,
    query
  }

  if (method === 'GET') {
    respond.end(JSON.stringify(resData))
  } else if (method === 'POST') {
    let postData = ''
    request.on('data', chunk => {
      postData += chunk.toString()
    })
    request.on('end', () => {
      resData.postData = postData
      respond.end(JSON.stringify(resData))
    })
  }
})


serve.listen(5050)
console.log(`访问地址：http://localhost:5050`)

```

### 搭建开发环境
* nodemon监测文件变化
* cross-env设置环境变量

### Node连接Mysql
```javascript
const mysql = require('mysql')
// 配置数据库
const connent = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'root',
  port: 3306,
  database: 'myblog',
})
// 连接数据库
connent.connect()
// 编写sql语句
// const sql = "insert into blogs(title, content,createtime, author, state) values ('apple', 'iphone12/iphone12pro/iphonemini',1603648888,'Tim', 1);"
const sql = "delete from blogs where id=4"
// 执行sql语句
connent.query(sql, (err, data) => {
  if (err) {
    console.log(err)
    return
  }
  console.log(data)
})
// 断开连接
connent.end()
```

### Node连接Redis
```javascript
const redis = require('redis')

const redisClient = redis.createClient(6379, 127.0.0.0)
redisClient.on('error', err => {
  console.log(err)
})
// redis.print传入后，当set完后打印是否正确
redisClient.set('key', 'value', redis.print)
redisClient.get('key', (err, val) => {
      if (err) {
        console.log(err)
        return
      }
      console.log(val)
      // 退出
      redisClient.quit()
    })
```

### 登录模块
* 前端输入username以及password给后端，后端通过校验，然后设置cookie返回给前端

```javascript
// 后端设置cookie
// 操作cookie, httpOnly 前端无法通过document.cookie获取
respond.setHeader('Set-Cookie', `username=${value.username}; path=/; httpOnly`)
```

前端用户输入用户名，密码给后端，后端校验用户名和密码，如果正确设置一个cookie返给前端，前端存储这个cookie后，当前端访问接口的时候，将这个cookie 发送给后端，然后，后端通过前端传递的cookie在session中找到对应的值，进行校验是否是当前的用户


* session的问题：session直接是变量，放在进程中的，进程内存有限，访问量过大；正是上线运行是多进程的，进程之间内存无法共享的，解决方法redis

### 日志
* 日志存储在文件中
* 日志为什么不存在redis中：日志文件非常大，日志对性能要求不高
* 日志为什么不存在mysql中：日志没有特定的结构，不需要连表查询，日志需要拷贝到各个服务器中运行













