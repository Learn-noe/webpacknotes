# 图片处理
## 1.图片处理
### 若在ｃｓｓ将图片以背景图的方式展示时，如：
    #img{
        background-img:url(../images/img.png);
    }此时需要使用file-loader，url-loader来处理.如果未使用loader将会报错
### 安装依赖 npm install --save-dev file-loader url-loader
    file-loader:解决引用路径的问题，拿background样式用URL引入背景图来说，webpack最终将各个模块打包成一个文件，因此我们样式中的url路径是相对于入口HTML文件的，而不是相对于原始css文件所在路径的这就会导致图片引入失败，这个问题是用file-loader来解决，file-loader可以解析项目中URL引入（不限于css）,根据配置，将图片拷贝到相应的路径，再根据我们的额配置，修改打包后文件引用路径，使之指向正确的文件
    url-loader:url-loader会将引入的图片编码，生成dataurl在打包到文件中，最终只需要引入这个文件就能访问图片了，当然图片较大，编码会被消耗性能，因此url-loader提供了一个limit参数，小于limit字节的文件会被转为dataurl大于limit的会使用file-loader进行Corp
    具体配置如下：
#### module：{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader']
            },{
                test:/\.(png|jpg|gif)/,
                use:[{
                    loader:'url-loader',
                    options:{
                        limit:5000,  //把小于50000 byte的文件打包成Base64的格式写入js
                    output:'images' //当大于是使用file-loader将图片打包到images目录下
                    }
                }]
            }
        ]
}
## 2.css分离后图片的路径处理
### 利用extract-text-webpack-plugin插件将css代码分离出来后，发现css图片路径并不正确。此时需要使用publicPath来解决
### publicPath:是在webpack.config.js文件中的output选项中，主要作用就是处理静态文件路径问题
### 具体配置：
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'[name].js',
        publicPath:"http://xxx.xxx.xxx.xxx:xxx/" 使用绝对路径
    }
## 处0理html中的图片
### 利用html-withimg-loader可以解决html文件中引入的<img>。
### 安装html-withimg-loader依赖：npm install --save-withimg-loader
### 具体配置如下
    module:{
        rules:[{
            test:/\.(html|htm)$/i,
            use:['html-widthimg-loader']
        }]
    }
## less文件的打包与分离
### 安装依赖：npm install --save-dev less
            npm install --save-dev less-loader
### 具体配置： module:{
                rules:[
                    {
                        test:/\.less$/,
                        use:[{
                            loader:'style-loader' //create style nodes from Js strings
                        },{
                            loader:'css-loader' //translates Css into CommonJs
                        },{
                            loader:'less-loader' //compiles Less to css
                        }
                    }
                ]
            }
    或者想要将less文件分离打包配置如下：
            modules:{
                roules:[
                    {
                        test:/.\less$/,
                        use:extractTextPlugin.extract({
                            use:[{
                                loader:'css-loader'
                            },{
                                loader:'less-loader'
                            }],//use style-loader in development
                            fallback:'style-loader'
                        })
                    }
                ]
            }
## sass文件的打包与分离
### 安装依赖包 因为sass-koader依赖于node-sass ,所以需要先安装node-sass
    npm install --save-dev node-sass
    npm install --save sass-loader
    注意：node-sass容易安装失败可以使用淘宝镜像进行配置npm或者使用cnpm
### 具体配置如下：
    modules:{
        roules:[
            {
                test:/\.scss$/,
                use:[{
                    loader:'style-loader' //create style node from Js strings
                },{
                    loader:'css-loader' //translates css into CommonJs
                },{
                    loader:'sass-loader' //complies sass to Css
                }]
            }
        ]
    }
    或者想要将sass文件分离打包配置如下
        modules:{
            roules:[
                {
                    test:/.\sass$/,
                    use:extractTextPlugin.extract({
                        use:[{
                                loader:'css-loader'
                        },{
                                loader:'sass-loader'
                        }],//use style-loader in development
                        fallback:'style-loader'
                    })
                }
            ]
        }
## 设置自动处理css3前缀
### css 3中有些属性需要加前缀，若自己查询并添加前缀会比较麻烦，可以使用postcss-loader给常用的css 3自动添加前缀
    postCss 是一个处理平台，可以实现很多功能
### 安装依赖: npm install --save-dev postcss-loader autoprefixer
### postcss配置，在根目录下（webpack.config.js同级），建立一个post.config.js文件post.config.js
    module.export={
        plugins:[require('autoprefixer')]
    }
### webpack.config.js配置
    modules:{
        roules:[
            {
                test:/\.css$/,
                use:[
                    {
                        loader:'style-loader'
                    },{
                        loader:'css-loader',
                        modules:true
                    },{
                        loader:'postcss-loader'
                    }
                ]
            }
        ]
    }
    将css打包分离配置如下：
        modules:{
            roules:[{
                test:/\/css$/,
                use:extractTextPlugin.extract({
                    fallback:'style-loader'
                    use:[
                        {loader:'css-loader',options:{importLoader:1}},
                        'postcss-loader'
                    ]
                })
            }]
        }
## 消除未使用的css
### 随着项目的进展，css也会越来越多，有时需求更改，带来了dom结构的更改，这是若不关注css样式，会造成css的冗余。使用Purrfify-webpack purify-css可以消除未使用的css，减少冗余。
### 安装purify-webpack purify-css依赖：npm install --save-dev purify-webpack purify-css
### 具体配置如下：
    const glob=require('glob');//因为需要同步检查html模板，所以需要引入node的glob对象。
    const PurifyCSSPlugin=require('purifycss-webpack');
    
    plugins:[
        new extractTextPlugin('css/index.css'),
        new PurifyCSSPligin({
            //这里配置了一个paths，主要是需要找html模板，purifycss根据这个配置会遍历你的文件，查找哪些css被使用了
            path:glob.sync(path.join(__dirname,'src/*.html'))
        })
    ]
    注意：这个插件必须配合extract-text-webpack-plugin插件使用