# Vue安装步骤
---
1. 安装node.js

2. npm切换淘宝镜像，以后记得使用cnpm而非npm

3. 可选】npm设置全局路径和缓存路径，设置后紧接着修改环境变量

4. 全局安装Vue-cli

5. 新建项目`vue init webpack [项目名]`，选择相关选项

6. 目录结构

   ![](https://img-blog.csdn.net/20181017085246392?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzI0MzQ2MjM1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

7. axios及跨域问题

   + [跨域问题](https://www.jianshu.com/p/1318132bdf96) [vue-axios问题](https://segmentfault.com/q/1010000010425538)

   + ```cmd
     npm install --save axios
     npm install --save vue-axios #封装的vue插件
     ```

   + 入口文件main.js里新增

     ```js
     import axios from 'axios'
     import VueAxios from 'vue-axios'
     Vue.use(axios,VueAxios)
     ```

   + 在package.json 同级创建vue.config.js，下面是例子

     ```js
     module.exports = {
         devServer: {
         proxy: {
           "/api": {
             target: "http://192.168.0.201:12386",   // 要请求的后台地址
             ws: true,    // 启用websockets
             changeOrigin: true,   // 是否跨域
             pathRewrite: {   
               "^/api": "/"     // 这里理解成用‘/api’代替target里面的地址，后面组件中我们掉接口时直接用api代替
             }
           },
           "/log": {
             target: "http://192.168.0.201:12368",     // 另一个要请求的地址
             ws: true,
             changeOrigin: true,
             pathRewrite: {
               "^/log": "/"
             }
           }
         }
       }
     }
     ```



## 错误总结

[webpack兼容问题](https://blog.csdn.net/u011618035/article/details/102621101)