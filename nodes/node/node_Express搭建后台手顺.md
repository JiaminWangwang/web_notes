## Node_Express搭建前后台手顺
[Express官网](https://www.expressjs.com.cn/)
**前提：已安装nodejs**
1. 全局安装express-generator(exExpress应用生成器)
    ```js
    npm i express-generator -g
    ```
2. 创建Express项目
    ```js
    express express-project
    ```
3. 下载项目依赖包
    ```js
    npm i
    ```
4. 启动项目
    ```js
    npm start
    ```
5. 在浏览器中输入以下地址：
    ```js
    // localhost:3000
    ```
#### **Express目录介绍**
+ bin
    + www -- 创建服务器文件
+ public -- 放前端代码，默认加载index.html
+ routes -- 服务器代码：接收前端ajax请求，并将请求做处理(向数据库插入/获取数据，返回状态)
+ views -- 后端写页面的文件(除非对首页渲染速度有特别高的要求，否则不用)
    + .jade -- jade是模版引擎，后端渲染好数据页面，速度快
+ app.js -- 服务器入口文件


6. 保存文件自动重启服务插件
    ```js
    npm i nodemon -g

    // 安装之后启动服务可以用命令
    nodemon app.js
    ```
    nodemon 命令报错
    [ 解决方案参考](https://blog.csdn.net/webjxy/article/details/121193543)