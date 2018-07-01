# vue-cli 多页面应用

基于vue-cli 2.9.3 版本`vue init webpack`命令生成的的应用，在此基础上进行改造成多页应用脚手架.在写多页的基础上完全与写Vue单页应用一样。

### 页面创建
在 `src/module`文件夹目录可以创建页面
如`index`文件夹,一个页面包括以下三个文件
```
app.html
app.js
App.vue
```
文件夹里必须包括一个.html 文件，.js 文件，.vue 文件作为入口文件
`npm run dev`的时候提示打开 localhost:8080 即可

## Build Setup

```bash
# install dependencies
npm install

# serve with hot reload at localhost:8080/index.html
npm run dev

# build for production with minification
npm run build
# 文件打包之后 可以启动本地服务查看
# serve with hot reload at localhost:2333
node server


```
### 改造了哪些东西
对Vue的webpack进行了改造
```
function getEntries (path) {
  let entries = {}
  glob.sync(path).forEach(entry => {
    if (/(module\/(?:.+[^.html]))/.test(entry)) {
      entries[RegExp.$1.slice(0, RegExp.$1.lastIndexOf('/'))] = entry
    }
  })
  return entries
}
```

```
 for (let pathname in entry) {
        let filename = pathname.replace(/module\//, '')
        let conf = {
          filename: filename === 'index'
            ? `${filename}.html`
            : `${filename}/index.html`, // `${filename}/index.html`,
          template: entry[pathname],
          inject: true,
          minify: {
            removeComments: true,
            collapseWhitespace: true,
            removeAttributeQuotes: true
            // more options:
            // https://github.com/kangax/html-minifier#options-quick-reference
          },
          // necessary to consistently work with multiple chunks via CommonsChunkPlugin
          chunksSortMode: 'dependency'
        }
        if (pathname in devWebpackConfig.entry) {
          conf.chunks = ['manifest', 'vendor', pathname]
          conf.hash = true
        }
        devWebpackConfig.plugins.push(new HtmlWebpackPlugin(conf))
  }
```