# Nodejs

## 模块部分
### http模块
```javascript
// 表示引入http
const http = require('http');
// request：获取客户端传递来的信息
// response：给浏览器的响应信息

http.createServer(function (request, response) {
  // 设置响应头
  response.writeHead(200, {'Content-Type': 'text/plain'});
  // 表示给页面输出一句话并结束相应 
  response.end('Hello World');
}).listen(8081); // 端口

console.log('Server running at http://127.0.0.1:8081/');
```

### fs模块
* 主要是文件操作模块
1. fs.stat：检测是文件还是目录
2. fs.mkdir：创建目录
3. fs.writeFile：创建并写入文件，重复执行会覆盖上次内容
4. fs.appendFile：追加文件，重复执行会叠加上次内容
5. fs.readFile：读取文件，二进制格式，需要使用toString()转换
6. fs.readdir：读取目录，以数组形式返回当前文件下的所有文件夹内容
7. fs.rename：重命名文件，移动文件
8. fs.rmdir：删除目录
9. fs.unlink：删除文件
10. fs.createReadStream：流方式读取文件
11. fs.createWriteStream：流方式写入文件

```javascript
const fs = require('fs');

// fs.stat：检测是文件还是目录
fs.stat('./package.json', (err, data) => {
  if (err) {
    console.log(err)
    return;
  }
  console.log(`文件：${data.isFile()}`)
  console.log(`目录：${data.isDirectory()}`)
})

// fs.mkdir：创建目录
fs.mkdir('./css', (err) => {
  if (err) {
    console.log(err)
    return;
  }
  console.log('创建成功')
})

// fs.writeFile：创建并写入文件，重复执行会覆盖上次内容
fs.writeFile('./css/html.html', 'hello byron', (err) => {
  if (err) {
    console.log(err)
    return;
  }
  console.log('创建写入文件成功')
})

// fs.appendFile：追加文件，重复执行会叠加上次内容
fs.appendFile('./css/base.css', '.next{color: red;}', (err) => {
  if (err) {
    console.log(err)
    return
  }
  console.log('追加成功')
})

// fs.readFile：读取文件，二进制格式，需要使用toString()转换
fs.readFile('./css/html.html', (err, data) => {
  if (err) {
    console.log(err)
    return
  }
  console.log(data)
  console.log(data.toString())
})

// fs.readdir：读取目录，返回当前文件下的所有文件夹内容
fs.readdir('./css', (err, data) => {
  if (err) {
    console.log(err)
    return
  }
  console.log(data)
})

// fs.rename：重命名文件，移动文件
fs.rename('./css/hello.css', './css/byron.css', (err) => {
  if (err) {
    console.log(err)
    return
  }
  console.log('重命名成功')
})
fs.rename('./css/hello.css', './data/hello.css', (err) => {
  if (err) {
    console.log(err)
    return
  }
  console.log('移动文件成功')
})

// fs.rmdir：删除目录
fs.rmdir('./a', (err) => {
  if (err) {
    console.log(err)
    return
  }
  console.log('删除目录成功')
})

// fs.unlink：删除文件
fs.unlink('./a/index.js', (err) => {
  if (err) {
    console.log(err)
    return
  }
  console.log('删除文件成功')
})

// 流方式读取文件
const readStrem = fs.createReadStream('./data/hello.txt')

let str = '',
    count = 0;
// 读取数据
readStrem.on('data', (data) => {
  str += data
  count++
})
// 结束读取
readStrem.on('end', () => {
  console.log('结束读取')
  console.log(str, count)
})
// 读取错误
readStrem.on('error', (err) => {
  console.log(err)
})

// 流方式写入文件
let str = ''

for (let i = 0; i < 500; i++) {
  str += '求购redmi手表白色\n';
}
const writeStrem = fs.createWriteStream('./data/out.txt')
writeStrem.write(str)
// 标记文件写完
writeStrem.end()
writeStrem.on('finish', () => {
  console.log('写入完成！')
})

// 管道流
const readStream = fs.createReadStream('./data/1.jpg')
const writeStrem = fs.createWriteStream('./hello/1.jpg')
readStream.pipe(writeStrem)
```

## 常用插件部分
* 目录生成插件：mkdirp

```shell
npm install mkdirp
```


## eggjs
### token权鉴
1. 安装egg-jwt

```shell
npm i egg-jwt
```

2. 配置egg-jwt

```javascript
// plugin.js
module.exports = {
  jwt: {
    enable: true,
    package: 'egg-jwt',
  },
};

// config.default.js
module.exports = appInfo => {
  // 关闭csrf，可以本地使用post
  config.security = {
    csrf: {
      enable: false,
    },
  };
  // 配置egg-jwt
  config.jwt = {
    secret: 'byron',	// 自定义token的加密条件字符串，可按各自的需求填写
  };

  return {
    ...config,
  };
};
```

3. 生成token

```javascript
// 一般在后端判断登陆成功后生成token，然后传给前端
const token = app.jwt.sign({
  // 需要存储的Token数据
  userID: data.username, 
  }, app.config.jwt.secret, {
  // token过期时间
  expiresIn: '1h',
});
```

4. 验证token

```javascript
// 前端：这里先补充一点，前端拿到token存储后，但访问需要权限页面时候，发送接口需要在请求头上添加token，举例：
axios({
  method: 'get',
  url: '/byron/home',
  headers: {
  // 切记 token 不要直接发送，要在前面加上 Bearer 字符串和一个空格
    'Authorization':`Bearer ${token}`
}...


// 后端：验证token，解析token后就可以对是否过期以及存储信息与库比较
const { userID, exp } = this.ctx.state.user;
```




















