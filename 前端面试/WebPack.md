## Webpack面试题
### 前端代码为何要进行构建和打包
### module chunk bundle分别是什么意思，有何区别
### loader和plugin的区别
* loader是文件加载器，能够加载资源文件，并对这些文件进行一些处理，如编译，压缩，最终打包到指定的文件中
* 在webpack广播中会出现许多事件，plugin会监听这些事件，在合适的时机通过webpack提供的api改变输出结果

### webpack如何实现懒加载
### webpack常见的性能优化
### babel-runtime和babel-polyfill的区别