# Express
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

# MongoDB
## 安装MongoDB
## MongoDB使用
+ 命令行操作
+ 可视化图形操作
+ 通过后端代码去操作MongoDB
### 1.命令行操作
1. 查看当前MongoDB中所有的数据库
```
show dbs
```
2. 查看当前指向的数据库
```
db
```
3. 新建/切换数据库
```
use 数据库名称
```
4. 查看当前数据库中所有的集合
```
show collections
```
5. 往集合中添加数据（如果该集合不存在，则会自动创建该集合）
```sql
db.集合名称.insert({name: '张三', age: '20', gender: '男})
```
6. 查看某个集合中所有的数据
```sql
db.集合名称.find()
db.集合名称.find().pretty()
```

### 2.可视化图形操作
1. Navicat Premium
2. 安装navicat

### 3.通过后端代码去操作MongoDB
**Mongoose**
是Nodejs中提供的一个用于便捷操作MongoDB的库。

### 下载
```
npm i mongoose --save
```

### 1.连接MongoDB
**将express项目与MongoDB连接**

```js
// 连接MongoDB 
const mongoose = require('mongoose');

//studentsSystem是数据库名称
const dbURI = 'mongodb://localhost:27017/studentsSystem'; 

mongoose.connect(dbURI, {useNewUrlParser: true, useUnifiedTopology: true});

mongoose.connection.on('connected', function() {
   console.log(dbURI + '数据库连接成功');
});
```
### 2.配置集合
```js
// 1.定义数据集合中的结构: 定义出集合中数据有哪些属性，属性值是什么类型
const { Schema, model } = require('mongoose);
```
```js
const usersSchema = new Schema({
    username: String,
    password: String
})
```
```js
// 2.定义数据集合的模型: 将schema和数据库中的集合关联起来
// model('模型名称', usersSchema, '数据库中的集合名称')
const usersModel('usersModel', usersSchema, 'users');
```
### 3.操作数据
+ 查询
    - 按条件查询
    ```js
    usersModel.find({username: '张三'})
    ```
    - 查询所有数据
    ```js
    usersModel.find()
    ```
+ 新增
```js
usersModel.create({username: '李四', password: '123456})
```
+ 删除
```js
usersModel.deleteOne({username: '张三'}) // 只删除一个名字是张三的数据
usersModel.deleteMany({username: '张三'}) // 删除所有名字是张三的数据
```
+ 修改
```js
usersModel.updateOne({_id: 1}, {username: '李四', password: '123456})
```
**_注：以上所有的方法都是异步方法，且这些方法的返回值都是Promise对象，因此需要通过await去等待结果。_**
```js
// 例子
router.post('/login', async function(req, res, next) {
    const result = await usersModel.find({username: '张三'});
})
```

## **后端服务器三层架构的概念**
### 1. 表现层：和前端传输数据
+ 接收前端发来的请求
+ 返回给前端数据
### 2. 服务层：处理业务逻辑(处理数据)
+ 从表现层拿到参数/数据，经过逻辑处理后发给持久层
+ 从持久层拿到数据，处理后发给表现层
### 3. 持久层：和数据库交换数据
+ 操作数据库，不做逻辑处理

**三个层级之前数据传递通过函数传参/return的方式**
