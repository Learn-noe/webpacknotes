# babel是什么
## babel是一个编译javascript的平台，它的强大之处表现在可以通过编译帮你打到：
### 1.使用下一代的javascript(es6,es7....)代码，及时当前浏览器没有完全支持
### 2.使用基于javascript进行拓展语言，比如React的jsx
### webpack配置babel
#### 安装依赖包：npm install --save-dev babel-core babel-loader babel-preset-react babel-preset-env
##### 1.babel-core：babel的核心包；
##### 2.babel-loader：babel的loader包；
##### 3.babel-preset-es2015：解析es6的包；
##### 4.babel-preset-env：解析es6的包；（官方最新推荐）
##### 5.babel-preset-react：解析React的JSX的包；
## 在webpack.config。js重配置一下代码
    module:{
        loader:[
            {
                test:/\.js$/,
                loader:'babel-loader',
                option:{
                    'preset':[latest],
                }
            }
        ]
    }
## 安装babel-loader
    npm install --save-dev babel-loader babel-core
## 因为配置了presets所以接下来需要安装 npm install --save-dev babel-preset-latest
## 示例如下：
    var htmlWebpackPlugin = require('html-webpack-plugin');
    var path = require('path');
    module.exports = {
        //文件入口
        entry: './src/app.js',
        //文件出口
        output: {
            path: __dirname + '/dist/',       //打包后的路径
            filename: 'js/[name].bundle.js'
        },
        module: {
            loaders: [
                {
                    test: /\.js$/,
                    loader: 'babel-loader',
                    //打包除这个文件之外的文件
                    exclude: path.resolve(__dirname,"./node_modules"),
                    //打包包括的文件
                    include: path.resolve(__dirname, "./src"),
                    options: {
                        'presets': ['latest']
                    }
                }
            ]
        },
        plugins: [
            //自动生成index.html文件
            new htmlWebpackPlugin({
                filename: 'index.html',
                template: "index.html"
            })
        ]
    }