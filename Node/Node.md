# Node.js
## 常用的cmd指令
* dir：列出当前目录下的所有文件
* cd 目录名：进入指定目录
* md 目录名：创建一个文件夹
* rd 目录名：删除一个文件夹

## npm命令
* npm -v：查看npm的版本
* npm version：查看所有模块版本
* npm search 包名：搜索包
* npm init ：构建package.json
* npm install / i 包名：安装包
* npm remove / r 包名：删除包
* npm install：下载当前项目所依赖的包
* npm install 包名 -g：全局安装包(全局安装的包一般为工具)
* npm info 包名：查看包的历史版本信息

## Node.js基础
### 模块化
* 核心模块：由node引擎提供的模块
* 文件模块：由用户自己创建的模块
* 在node执行代码后会自动将代码封装到以下函数中
```javascript
function (exports, require, module, __filename, __dirname) {}
// exports:该对象用来将变量或函数暴露到外部
// require:函数，用来引入外部的模块
// module:代表当前模块的本身，exports就是module的属性
// __filename:当前模块的完整路径
// __dirname:当前模块所在文件夹的完整路径
```

#### 加载require
* 通过require()函数来引入其他模块,require()传递一个文件的路径作为参数，node会根据路径自动引入外部模块，使用require()引入模块后，该函数会返回一个对象，这个对象代表的是引入的模块
* 语法
```javascript
const 自定义变量名 = require('模块')
```

* 作用
>执行加载模块中的代码
>的到就被加载模块中exports导出接口对象

* 模块操作路径
```javascript
//去磁盘根目录的data中查找index
require('/data/index.js')
require('data/index.js')
//相对于当前文件的data中查找index
require('./data/index.js')
```

#### 导出exports
* 在node中一个文件就是一个模块
* 通过exports来向外部暴露变量和方法，只需要将需要暴露给外部的变量或方法设置为exports的属性即可
* 导出单个成员（可以导出函数，字符串）,执行多次module.exports会覆盖
```javascript
//export导出对象在对象中
exports.a = '123'
exports.b = 'abc'
exports.c = {
  foo: 'bar'
}
//module.exports覆盖之前的导出
module.exports = function(){}
```

* exports与module.exports区别
>如果使用module.exports那么exports会自动失效

#### ip与端口
* ip地址用来定位计算机
* 端口号用来定位具体的应用程序，0 - 65536
* Content-Type：告知我给对方发送数据的内容格式
>text/plain：普通文本
>text/html：html格式文本

### 函数部分
#### Buffer(缓冲区)
* Buffer的结构和数组很像，操作方法也与数组类似，数组中不能存放二进制文件，而Buffer专门来存储二进制数据
* 在Buffer中存储的都是二进制数据，但是在显示时都是16进制的形式显示 00-ff
* Buffer.from(str)：将一个字符串保存到buffer中
* Buffer.alloc(20)：创建一个指定字节的Buffer,长度确定后不能修改
* Buffer.allocUnsafe(20)：创建一个指定字节的Buffer,长度确定后不能修改，可能存在敏感数据（不清除开辟空间的数据）
* buf.toString()：缓冲区数据转换为字符串
```javascript
//将一个字符串保存到buffer中
var buf = Buffer.from(str);
// 创建一个指定字节的Buffer,长度确定后不能修改
const buf1 = Buffer.alloc(20);
const buf2 = Buffer.allocUnsafe(20);
```

### fs文件模块
* 通过node来操作系统中的文件，使用文件系统，需要先引入fs模块，fs是核心模块

#### 文件写入部分
##### 同步方法
* fs.openSync(path, flage, [mode])：同步打开文件,返回一个文件描述符作为结果，我们可以通过该描述符对文件进行操作
>path: 要打开文件的路径
>flage：打开文件要操作的类型 r：只读 w：可写
>mode 设置文件的操作权限，一般不传

* fs.writeSync(fd, string[, position[, encoding]])：同步编写文件
>fd：文件描述符，需要传递要写入的文件的描述符
>string：要写入的内容
>position：写入的起始位置
>encoding：写入的编码，默认utf-8

* fs.closeSync(fd)：同步关闭文件
>fd：要关闭的文件的描述符

```javascript
const fs = require('fs');
// 打开文件
const fd = fs.openSync('hello.txt', 'w');
// 对文件进行操作
fs.writeSync(fd, "hello byron!", 2);
// 关闭文件
fs.closeSync(fd);
```

##### 异步方式
* fs.open(path[, flags[, mode]], callback)：异步打开文件,异步调用方法，结果都是通过回调函数的参数返回的,回调函数有两个参数：err：错误对象，如果没有错误则为null,fd：文件的描述符
* fs.write(fd, string[, position[, encoding]], callback) ：用来异步写入一个文件
* fs.close(fd, callback)：异步关闭文件
```javascript
const fs = require('fs');

fs.open('hello2.txt', 'w', function(err, fd){
  // 判断是否有错
  if(!err) {
    // 如果没错进行文件写入
    fs.write(fd, '这个是异步文件写入', function(err){
      if(!err){
        console.log('文件写入成功！');
      }
      //关闭文件
      fs.close(fd, function(err){
        if(!err){
          console.log('文件已关闭！');
        }
      })
    })
  }
})
```

##### 简单文件写入 - 常用
* fs.writeFile(file, data[, options], callback)
>file：要操作的文件路径
>data：要写入的数据
>options：选项，可以对文件进行编码，模式，读写(flag:r,w,a)的设置
>callback：当写入完成以后执行的函数

```javascript
  const fs = require('fs');
  fs.writeFile('hello3.txt','这是简单方式写入',function(err){
    if(!err){
      console.log('写入成功');
    }else{
      console.log('写入失败');
    }
  })
```

##### 流式文件写入 - 适合大文件写入
* fs.createWriteStream(path[, options]):创建一个可写流
>path：文件路径
>options：配置的参数

```javascript
const fs = require('fs');
const ws = fs.createWriteStream('hello4.txt');
// on(事件字符串，回调函数)
//   可以为对象绑定一个事件，类似jq
// once(事件字符串，回调函数)
//   可以为对象绑定一次性事件，该事件将会在触发一次以后自动失效
ws.once('open',function(){
  console.log('流式文件打开了！')
})
ws.once('close',function(){
  console.log('流式文件关闭了！')
})
ws.write('通过流式文件写入');
ws.write('锄禾日当午');
ws.write('汗滴禾下土');
// 关闭流
ws.close();
```

#### 文件读取部分
##### 简单文件读取
* fs.readFile(path[, options], callback) | fs.readFileSync(path[, options])
>path：要读取文件的路径
>options：读取的选项
>callback：回调函数，通过回调函数读取到内容返回

```javascript
const fs = require('fs');
fs.readFile('hello2.txt', (err, data) => {
  if(!err){
    fs.writeFile('hello5.txt',data,(err) => {
      if(!err){
        console.log('文件写入成功');
      }
    }) 
  }
})
```

##### 流式文件读取
* fs.createReadStream(path[, options])
```javascript
const fs = require('fs');
// 创建一个可读流
const rs = fs.createReadStream('hello.txt');
// 创建一个可写流
const ws = fs.createWriteStream('hello6.txt');

rs.once('open', () =>{
  console.log('可读流开始');
})
rs.once('close', () =>{
  console.log('可读流结束');
})
ws.once('open', () =>{
  console.log('可写流开始');
})
ws.once('close', () =>{
  console.log('可写流结束');
})
// pipe()可以将可读流中的内容，直接输出到可写流
rs.pipe(ws);
```

#### 文件其他方法
* fs.existsSync(path)：检查一个文件是否存在,返回boolean
```javascript
const flag = fs.existsSync('hell.txt');
console.log(flag);
```

* fs.stat(path[, options], callback) | fs.statSync(path[, options]) ：获取文件状态，这个对象中保存了当前对象状态的相关信息
```javascript
fs.stat('hello.txt', (err, stat) => {
  if(!err){
    //size：文件大小
    //isFile():是否是一个文件
    //isDirectory():是否是一个文件夹
    console.log(stat.isDirectory());
  }
})
```

* fs.unlink(path, callback) | fs.unlinkSync(path):删除文件
```javascript
fs.unlink('hello6.txt', (err) => {
  if(!err){
    console.log('删除成功');
  }
})
```

* fs.readdir(path[, options], callback(err,files)) | fs.readdirSync(path[, options])：读取一个目录的目录结构
```javascript
fs.readdir('.', (err, files) => {
  if(!err){
    console.log(files);
  }
})
```

* fs.truncate(path[, len], callback) | fs.truncateSync(path[, len])：截断文件，将文件修改为指定大小
```javascript
fs.truncate('hello.txt', 5, (err) => {
  if(!err){
    console.log('截断成功');
  }
})
```

* fs.mkdir(path[, options], callback) | fs.mkdirSync(path[, options])：创建一个文件夹
```javascript
fs.mkdir('hello6.txt', (err) => {
  if(!err){
    console.log('创建成功')
  }
})
```

* fs.rmdir(path[, options], callback) | fs.rmdirSync(path[, options])：删除一个文件夹
```javascript
fs.rmdir('hello6.txt',(err) => {
  if(!err){
    console.log('删除成功')
  }
})
```

* fs.rename(oldPath, newPath, callback) | fs.renameSync(oldPath, newPath)：对文件重新命名，oldPath：旧的路径，newPath：新的路径，callback：回调函数
```javascript
fs.rename('hello5.txt','../hello5.txt', (err) => {
  if(!err){
    console.log('修改成功');
  }else {
    console.log(err);
  }
})
```

* fs.watchFile(filename[, options], listener)：监视文件的修改，filename：要监事的文件，options：配置选项，listener：回调函数，当文件发生变化时
```javascript
fs.watchFile('hello.txt',(curr,prev) => {
  // curr:当前文件状态，stats对象
  // prev：修改前文件的状态stats对象
  console.log(curr.size)
  console.log(prev.size)
})
```

### http模块
* 核心模块：const http = require('http');
* 创建web服务器：使用http.createServer()方法创建web服务器，返回Sever实例
* 请求事件：request请求事件，当客户端访问时，调用回调函数，request回调函数有两个参数
>request：请求对象，获取客户端的一些请求信息，eg：request.url:获取当前请求地址
>response：响应对象，eg：response对象可以使用write向客户端发送数据，但最后一定要使用end结束响应

* 启动服务：server.listen(5000, () => {})绑定端口号，启动服务器
* 简易完成实例
```javascript
// 加载核心模块
const http = require('http');
// 使用http.createServer()方法创建web服务器，返回Sever实例
const server = http.createServer();
// request请求事件，当客户端访问时，调用回调函数
  // request回调函数有两个参数
  //   request：请求对象，获取客户端的一些请求信息，
  //   response：响应对象
server.on('request', (request, response) => {
  console.log(`当前请求地址为：${request.url}`);
  // response对象可以使用write向客户端发送数据，但最后一定要使用end结束响应
  response.write('hello byron');
  response.end();
})
//绑定端口号，启动服务器
server.listen(5000, () => {
  console.log('服务器启动成功，通过 http://127.0.0.1:5000/ 来访问！');
})
```

* 当在解析的过程中，如果发现：link,script,img,iframe,video,audio等带有 src 或者 href（link） 属性标签（具有外链的资源）的时候，浏览器会自动对这些资源发起新的请求。

### path模块
#### 常用方法
* 获取路径中的文件名字
```javascript
 path.basename('c:/a/v/x/index.js','.js')
//'index'
```

* 获取路径名字
```javascript
 path.dirname('c:/a/v/x/index.js')
//'c:/a/v/x'
```

* 获取扩展名称
```javascript
 path.extname('c:/a/v/x/index.js')
//'.js'
```

* 判断路径是否是绝对路径
```javascript
 path.isAbsolute('c:/a/v/x/index.js')
//true
 path.isAbsolute('./v/x/index.js')
//false
```

* 获取基础信息
```javascript
path.parse('c:/a/v/x/index.js')
{
  root: 'c:/',		//根目录
  dir: 'c:/a/v/x',	//目录
  base: 'index.js',	//包含后缀名的文件名
  ext: '.js',		//后缀名
  name: 'index'		//不包含后缀名的文件名
}
```

* 拼接路径
```javascript
//可以添加多个参数进行拼接
path.join('c:/a', 'b')
// 'c:\\a\\b'
```

### Node中的其他成员
* 在每个模块中，除了`require`，`exports`的模块相关的API之内，还有两个特殊的成员
* `__dirname`可以用来动态获取当前文件模块所属目录的绝对路径
* `__filename`可以用来动态获取当前文件的绝对路径


### 热更新nodemon
* 安装
```shell
npm install -global  nodemon
```

* 使用
```shell
node app.js	//安装前
nodemon app.js	//安装后
```


## 模板部分
### art-template模板引擎
* 安装模板
```javascript
npm install art-template
```

* 模板引擎不关系字符串的内容，之关系自己能认识的模板标记语法：{{}}
* 基本用法
>1、模板文件
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
<head>
  //别忘记引入模板js文件
  <script src="../node_modules//art-template//lib//template-web.js"></script>
  <title>{{ title }}</title>
</head>
<body>
  <h1>Index of /static</h1>
  <table>
    {{ each files }}
    <tr>
      <td valign="top"><img src="/icons/folder.gif" alt="[DIR]"></td>
      <td><a href="css/">{{ $value }}/</a> </td>
      <td align="right">{{time}} </td>
      <td align="right"> - </td>
      <td>&nbsp;</td>
    </tr>
    {{ /each }}
  </table>
</body>
</html>
```

>2、模块文件
```javascript
//获取对应的核心模块
const http = require('http');
const template = require('art-template');
const fs = require('fs');
const time = new Date();
//建立服务
const server = http.createServer();
//请求服务
server.on('request', (request, response) => {
  // 读取文件
  fs.readFile('./template.html', (err, data) => {
    if(err){
      console.log('文件读取失败！！！')
    }
    // 读取目录
    fs.readdir('./dir', (err, files) => {
      if(err) {
        console.log('目录读取失败！！!')
      }
      var htmlStr = template.render(data.toString(), {
        title: 'byron',
        files,
        time
      })
      // 发送模板
      response.end(htmlStr)
    })
  })

})
//端口设置
server.listen('5000', () => {
  console.log('服务已开启，请访问：http://127.0.0.1:5000 来访问。。。')
})
```


* 服务端渲染和客户端渲染的区别
>客户端渲染不利于 SEO 搜索引擎优化
>服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的


## 框架部分
### Express框架
* 安装
```shell
npm install express --save
```

* 初始化express
```javascript
//引入模块
const express = require('express');
//创建服务
const app = express();
//开放文件
app.use('/public/', express.static('./public/'));
//监听服务
app.listen('5000', () => {
  console.log('服务已开启，访问： http:127.0.0.1:5000')
})
```
### Express脚手架
* 安装
```shell
# 全局安装 express-generator (安装它的目的 是为了 运行 express命令 )
npm i express-generator -g
# 使用express创建名为demo的项目
express demo -e
# 创建项目后记得进入项目安装依赖
npm install
# 启动项目
npm start
```

#### Express使用art-template模板引擎
* 安装
```shell
npm install --save art-template
npm install --save express-art-template
```

* 配置
```javascript
//配置express-art-template模板
app.engine('html', require('express-art-template'));
//配置默认目录
app.set('views', 新目录)
```

* 使用
```javascript
app.get('/', (req, res) => {
  //render默认选在views下的文件
  res.render('index.html', {
    title: 'Express'
  })
  //重定向
  res.redirect('/')
})
```

#### Express获取post数据
* Express获取post数据需要使用第三方插件
* 安装body-parser中间件
```shell
npm install --save body-parser
```

* 配置body-parser中间件
```javascript
//添加body-parser模块
const bodyParser = require('body-parser');
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }));
// parse application/json
app.use(bodyParser.json());
```

* 使用body-parser中间件
```javascript
app.post('/post', (req, res) => {
  //通过req.body获取post信息
  const data = req.body;
  data.dateTime = Date();
  comments.unshift(data);
  //重定向
  res.redirect('/')
})
```

#### Express使用路由
* 使用方法
>创建路由文件router.js
```javascript
const fs = require('fs');
//引入express插件
const express = require('express');
//创建一个路由
const router = express.Router()
//路由挂在到router中
router.get('/', (req, res) => {
  res.render('hello')
})
//导出路由
module.exports = router;
```

>在入口文件进行引入
```javascript
const express = require('express');
//引入路由
const router = require('./router');
const app = express();
app.engine('html', require('express-art-template'))
app.use('/node_modules/', express.static('./node_modules/'))
app.use('/public/', express.static('./public/'))
//路由挂在到app上
app.use(router);
app.listen('5000', () => {
  console.log('服务已连接：http:127.0.0.1:5000');
})
```

#### Express-Session
* 安装
```shell
npm install express-session
```

* 使用插件
```javascript
const express = require('express')
const session = require('express-session')

const app = express()

app.use(session({
  // 配置加密字符串，它会在原有加密基础之上和这个字符串拼起来去加密
  // 目的是为了增加安全性，防止客户端恶意伪造
  secret: 'blog',
  resave: false,
  // 无论你是否使用 Session ，我都默认直接给你分配一把钥匙
  saveUninitialized: false
}))

//添加session
req.session.user = "value"
//查看session
req.session.user
```

#### Express-Session使用mongodb数据库进行持久化
* 简介：在mongodb数据库中存储session数据
* 安装
```shell
npm install connect-mongo
```

* 使用插件
```javascript
const MongoStore = require('connect-mongo')(session)

app.use(session({
  // 配置加密字符串，它会在原有加密基础之上和这个字符串拼起来去加密
  // 目的是为了增加安全性，防止客户端恶意伪造
  secret: 'blog',
  resave: false,
  // 无论你是否使用 Session ，我都默认直接给你分配一把钥匙
  saveUninitialized: false,
  cookie: { maxAge: 60 * 60 * 1000 },//设置cookie时间
  store: new MongoStore({   //将session存进数据库  用来解决负载均衡的问题
    url: 'mongodb://127.0.0.1/session', //将缓存数据放入数据库中
    touchAfter: 24 * 3600 //通过这样做，设置touchAfter:24*3600，您在24小时内
    //只更新一次会话，不管有多少请求(除了在会话数据上更改某些内容的除外)
  })
}))
```

#### MySQL对session持久化express-mysql-session
* 安装，看文档
```shell
npm install express-mysql-session
```

### Koa框架


## MongoDB
### 启动与关闭数据库
* 启动
```shell
# 执行mongod命令后，会默认在指定的盘符下data/db作为存储数据目录
# 如果没有data/db目录可以手动创建
mongod
```

* 修改默认的数据存储目录
```shell
mongod --dbpath=数据存储目录路径
```

### 连接数据库
```shell
# 默认连接本机mongoDB服务
mongo
# 退出
exit
```

### 基本命令
* `show dbs` ：查看显示当前数据库
* `db`：查看当前操作数据库
* `use数据库名`：切换到指定数据库，如果没有会新建

### Node中使用mongodb（mongoose插件）
#### 基本使用
* 安装mongoose
```shell
npm install mongoose
```
* 基本使用
```javascript
//引入模块
const mongoose = require('mongoose')
//1、创建表结构
const Schema = mongoose.Schema;
//连接数据库itcast
//指定连接的数据库不需要存在，当你插入第一条数据的时候就会自动创建出来
mongoose.connect('mongodb://localhost/itcast');
//2、设计集合及结构（表结构），字段名称就是表结构中属性名称，值
var userSchema = new Schema({
  username: {
    type: String,
    required: true //必须有
  },
  password: {
    type: String,
    required: true
  },
  email: {
    type: String
  },
  time: {
    type: Date,
    default: Date.now
  }
});
//3、将文档结构发布为模型
// 第一个参数：传入单数名词作为表名信息，mongoose会自动将表明信息转换为小写复数 eg：User => users
// 第二个参数：架构（表结构）Schema
const User = mongoose.model('User', userSchema)
//4、当创建好模型后，就可以使用该构造函数对表进行操作

```

#### 新增数据
```javascript
//插入数据
const user = new User({
  username: 'admin',
  password: '123456',
  email: '123456@gmail.com'
})
user.save().then((value) => {
  console.log(`value:${value}`)
}).catch((reason) => {
  console.log(`reason:${reason}`)
})
```

#### 查询数据
* 查询所有
```javascript
User.find().then((value) => {
  console.log(`查询成功：${value}`)
}).catch((reason) => {
  console.log(`查询失败：${reason}`)
})
```

* 按条件查询所有
```javascript
//根据条件查询 返回的是数组
User.find({
  username: 'byron'
}).then((value) => {
  console.log(`查询成功：${value}`)
}).catch((reason) => {
  console.log(`查询失败：${reason}`)
})
```

* 按条件查询单个数据
```javascript
//只找匹配的第一个符合条件的,如果没有条件查找第一条数据，返回一个对象
User.findOne({
  username: 'byron'
}).then((value) => {
  console.log(value)
}).catch((reason) => {
  console.log(`查询失败：${reason}`)
})
```

#### 删除数据
```javascript
User.deleteOne({
  username: 'byron'
}).then((value) => {
  console.log(`删除成功：${value}`)
}).catch((reason) => {
  console.log(`删除失败：${reason}`)
})
```

#### 更新数据
```javascript
//更新数据.findByIdAndUpdate：返回更新前数据
User.findByIdAndUpdate('5ea0577aafeab028140e3e14', {
  password: 'byron'
}).then((value) => {
  console.log(`更新成功：${value}`)
}).catch((reason) => {
  console.log(`删除失败：${reason}`)
})
```

#### 基本API
* find：查询多条文档
* findById：根据id查询单条文档
* findOne： 根据条件查询单条文档
* findByIdAndUpdate：根据id进行更新
* findOneAndUpdate：根据条件更新单条文档
* findByIdAndRemove：根据id进行删除
* findOneAndRemove：根据条件删除单条数据










