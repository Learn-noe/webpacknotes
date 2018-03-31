# 文件打包
## 1.css打包简单配置
### css打包要依赖style-loader和css-loader 来解析。首先安装这两个loader
### 安装依赖包： npm install style-loader css-loader --save-dev
    编写css文件 "./src/css/index.css"
    body{
        background-color:red;
        color:white;
    }
    在入口文件 "。、src/entry.js中引入css文件"
     import css from './css/index.css'
     
     style-loader:style-loader是用来处理css文件中的url()等
     css-loader:css-loader是用来将css插入到页面style标签
     配置如下：
        module:{
            rules:[
                {
                    test:/\.css$/,
                    use:['style-loader','css-loader']
                }
            ]
        }
## 2.单独打包css
### 上述方法是将css以style标签的形式插入到head标签下，如果想将css单独提取出来需要使用extract-text-webpack-plugin插件
    虽然extract-text-webpack-plugin可以完成css代码提取，但是webpack官方不建议这样做，他们认为css就应该打包在javascript中以减少http的请求
### 安装eatract-text-webpack-plugin 依赖： npm install --save-dev extract-text-webpack-plugin
    引入：
        const extractTextPlugin=erquire("extract-text-webpack-plugin");
### 具体配置：、
    plugins:[
        new extractTextPlugin("/css/index.css") //  /css/index.css是分离后的路径
    ]
    modules:{
        rules:[
            test:/\.css$/,
            use:extractTextPlugin.extract({
                fallback:"style-loader",
                use:"css-loader"
            })
        ]
    }