## 一个基于vue-cli修改的多页面脚手架

> 主要是提供了多页面的编写，以及打包生成。这其中并不包括多页面的路由。


## 目录结构

```bash
├── src  # main folder
│   ├── assets  # common assets folder
│   │   ├── img
│   │   │   └── logo.png
│   │   ├── js
│   │   └── css
│   ├── components # common components folder
│   │   └── modal.vue
│   ├── common # common content folder
│   │   └── js
│   │       └── common.js
│   └── views  # pages
│       ├── index
│       │   ├── index.html 
│       │   ├── index.js 
│       │   └── index.vue
│       └── test 
│           ├── test.html 
│           ├── index.js 
│           └── index.vue
├── LICENSE
├── .babelrc         
├── package.json
└── README.md
```

## 初始化

``` bash
$ git clone git@github.com:yanluohao/vue-cli-multiple-pages.git
$ npm install
$ npm run dev
```

## 版本

> vue-cli v2.8.2

> webpack v3.10.0

> node v6.9.1

## 修改的内容

> utils.js
```js
// 多页配置
var glob = require('glob')
var HtmlWebpackPlugin = require('html-webpack-plugin')
var PAGE_PATH = path.resolve(__dirname, '../src/views')
var merge = require('webpack-merge')

...

//多入口配置
//获取views文件夹下，每个页面下的index.js作为页面入口，故每个页面下都必须有index.js
exports.entries = function () {
    var entryFiles = glob.sync(PAGE_PATH + '/*/index.js')
    var map = {},
        tmp = [],
        pathname = '';
    entryFiles.forEach((filePath) => {
        var filename = filePath.substring(filePath.lastIndexOf('\/') + 1, filePath.lastIndexOf('.'))
        tmp = filePath.split('/').splice(-4)
        map[tmp[2] + '/' + filename] = filePath
    })
    return map
}

//多页面输出配置
//读取views文件夹下的对应每个页面的html后缀文件，然后放入数组中
//如果想要更深的定制或者修改，建议大家看一下CommonsChunkPlugin
//推荐一个基础的 https://segmentfault.com/q/1010000009070061
exports.htmlPlugin = function () {
    let entryHtml = glob.sync(PAGE_PATH + '/*/*.html')
    let arr = []
    entryHtml.forEach((filePath) => {
        let jsPath = '',
            tmp = [];
        let filename = filePath.substring(filePath.lastIndexOf('\/') + 1, filePath.lastIndexOf('.'))
        tmp = filePath.split('/').splice(-4)
        jsPath = tmp[2] + '/' + 'index'
        let conf = {
            template: filePath,
            filename: filename + '.html',
            chunks: ['manifest', 'vendors', jsPath],
            inject: true
        }
        if (process.env.NODE_ENV === 'production') {
            conf = merge(conf, {
                minify: {
                    removeComments: true,
                    collapseWhitespace: true,
                    removeAttributeQuotes: true
                },
                chunksSortMode: 'dependency'
            })
        }
        arr.push(new HtmlWebpackPlugin(conf))
    })
    return arr
}
```

剩下的修改在base.conf,dev.conf,prod.conf三个js文件中。文件中的修改，用了备注和注释。这里就不再具体贴了。
想要更深入的定制化，需要自己参考其他人的方法。
