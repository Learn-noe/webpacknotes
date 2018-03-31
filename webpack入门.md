# 简介
## webpack可以看作是模块打包机：它做的事情是，分析项目结构，找到javascript模块以及其他一些浏览器不能直接运行的拓展语言（R如less、sass、ts等），并将其转换为和打包为合适的格式提供浏览器使用。在3.0出现后，webpack还肩负起了优化项目的责任：即webpack完成度额任务有：
    1.打包：可以把多个javascript文件打包成一个文件，减少服务器和下载宽带
    2.转换：吧拓展语言转换成普通的javascript，并让浏览器顺利运行
    3.优化：前端变得越来越复杂，性能也会遇到问题，而webpack也开始肩负起了优化和提升性能的责任
# 环境搭建
### 1.项目目录 webpackdemo/src webpackdemo/dist
### 2.进入项目目录初始化项目 npm init
### 3.安装webpack 1.npm install -g webpack //全局安装webpack可以使用webpack命令
                  2.npm install --save-dev webpack 安装测试依赖
### 4.若不全局安装webpack则需在package.json中配置，使用npm run build命令运行webpack，配置如下：
    "scripts": {
        "build": "webpack"
    },
# 测试 
### 安装依赖 npm install
### 运行webpack npm run build  不带watch
                npm run server 带watch
# webpack配置
##  webpack的配置文件是webpack.config.js，这个文件需要自己在项目根目录下手动建立。建立好后对其配置，首先了解一下webpack.config.js的基本结构：
    module.exports = {
        entry: {}, 
        output:{},
        module:{},
        plugin:[],
        devServer:{}，
        watchOptions：{}
    }
### entry:入口文件配置信息，可以是单一入口，也可以是多入口
### output:出口文件配置信息，在webpack以后，支持多出口配置
### module:模块陪配置信息，主要是解析sass、less\图片转换、将拓展语言转换成普通的javascript等
### plugins:插件配置信息，根据需求，配置不同插件
### devServer:开发服务器配置，该服务器是一个小型的Express服务器，主要有两个功能：为静态资源提供服务以及自动刷新和热替换
### watchOptions:watch配置，自动刷新和热替换