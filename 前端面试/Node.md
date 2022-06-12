## Node面试题
### 介绍一下npm模块安装机制，为什么输入npm install 就可以自动安装对应的模块
* 当实用npm安装模块时会把依赖信息安装到package.json中，当使用npm install后自动到package.json上找到依赖项进行安装模块