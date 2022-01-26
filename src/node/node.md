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

## MongoDB
### 安装MongoDB
### MongoDB使用
#### 命令行操作
1. 查看当前MongoDB中所有的数据库
`show dbs`
2. 查看当前指向的数据库
`db`
3. 新建/切换数据库
`use 数据库名称`
4. 查看当前数据库中所有的集合
`show collections`
5. 往集合中添加数据（如果该集合不存在，则会自动创建该集合）
`db.集合名称.insert({name: '张三', age: '20', gender: '男})`
6. 查看某个集合中所有的数据
`db.集合名称.find()`
`db.集合名称.find().pretty()`

## navicat
1. 安装navicat