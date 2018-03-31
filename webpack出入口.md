# 入口文件配置-entry选项
## 入口起点指示webpack应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack会找到哪些模块和库是入口起点（直接和间接）依赖的
### 例如：1、entry: "./src/entry.js"
            2、entry: ['./src/entry.js', './src/entry2.js']
            3、entry: {
                entry: './src/entry.js'
            }
            4、entry: {
                entry: './src/entry.js', // 对象属性名随意命名
                entry2: './src/entry2.js'
            }
### 出口文件配置-output选项 出口文件告诉webpack在哪里输出它所创建的文件，以及如何命名这些文件
### 在webpack中配置output属性是一个对象，其必须包含 filename、path两个属性。
        filename：用于输出文件的文件名
        path：目标输出目录的绝对路径
### 例如：output: {
        filename: 'bundle.js',
        path: "/home/webpacktest/dist"
    }
### path也可以使用node的path对象来处理路径，在webpack.config.js的头部来引入path,代码如下：
        const path = require('path');

        output: {
            filename: 'bundle.js',
            path: path.resolve(__dirname, 'dist') path.resolve(__dirname, 'dist')会获取到项目的绝对路径。
        }
## 多出口多入口文件
### entry: {
        entry: './src/entry.js',
        entry2: './src/entry2.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].js'
    }

## 服务配置-devServer选项
### webpack-dev-server是webpack官方提供的一个小型的express服务器。使用它可以为webpack打包生成的资源文件提供web服务，以及自动刷新和热替换。 
    要执行webpack-dev-server需要先安装，使用 npm install webpack-dev-server --save-dev 安装webpack-dev-server。安装完进行配置，最简单的devServer只需要四项：
    devServer: {
    contentBase: path.resolve(__dirname, 'dist'),
    host: 'localhost,
    compress: true,
    port: 3000
}
contentBase:配置服务器基本运行路径。告诉服务器从哪里提供内容。只有你想要提供静态文件时才需要。
host:服务器运行地址，建议使用本机IP默认为localhost,如果你希望服务器外部可访问指定为0.0.0.0
compress:服务器压缩选项，一般这职位开启
port:服务器运行端口