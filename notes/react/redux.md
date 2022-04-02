> [redux中文文档](http://www.redux.org.cn/)

### redux理解
用于做状态管理的JS库，
集中式状态管理，共享状态

### redux的三个核心概念
- redux原理图[图片](../../img/redux_%E5%8E%9F%E7%90%86%E5%9B%BE.png)
1. action
    - 动作的对象
        - type: 标识属性，值为字符串，唯一，必要属性
        - data: 数据属性，值类型任意，可选属性
    ```js
    {type: 'ADD_STUDENT', data:{name: 'tom', age: 18}}
    ```
2. reducer
    - 用于初始化状态、加工状态
    - 加工时，根据旧的state和action, 产生新的state的纯函数
3. store
    - 将state、action、reducer联系在一起的对象