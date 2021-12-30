Webpack

模块化打包工具，通过Loader编译兼容，编译转换.vue、.jsx、.less文件，通过plugin实现代码压缩，按需加载，给webpack.config.js的output添加chunkFilename设置模块名字，在需要按需加载的文件里写上impot（'./b'）.then 最后配置publicPath基础路径

babel 了解过AST抽象语法树，