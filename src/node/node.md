## Express
Express 是一个基于node.js的服务端框架

## Express应用程序生成器
名称: express-generator;
作用: 可以帮助开发者快速创建一个express应用。

## Express目录介绍
+ bin
    + www -- 创建服务器文件
+ public -- 放前端代码，默认加载index.html
+ routes -- 服务器代码：接收前端ajax请求，并将请求做处理(向数据库插入/获取数据，返回状态)
+ views -- 后端写页面的文件(除非对首页渲染速度有特别高的要求，否则不用)
    + .jade -- jade是模版引擎，后端渲染好数据页面，速度快
+ app.js -- 服务器入口文件
