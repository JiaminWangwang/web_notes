## react-redux
[react-redux模型图](../../img/react-redux%E6%A8%A1%E5%9E%8B%E5%9B%BE.png)

- 连接容器组件与UI组件
    - connect(mapStateToProps, mapDispatchToProps)
    - mapDispatchToProps: 两种值
        - 可以是函数
        - 可以是对象

- Provider
    会将store传给包裹的容器内部的每个容器组件

- 数据共享（109 ~ 111）------------------------  补代码
    - 多个reducer时 --- 合并reducer: combineReducers({reducer1, reducer2, ...})

- 纯函数 ***重点***
    - 一个知识点：
        - 数组方法：shift、unshift、push、pop等操作原数组的数组方法不能在reducer中使用，
            会造成数组改变到时画面不更新的bug，因为react底层会将两次数组进行浅比较，发现
            两次数组都是同一指针，不会进行画面更新。
        - reducer更新时使用数组展开运算符，返回新数组
            ```js
            [newItemObj, ...initArray]
            ```
    - 什么是纯函数？：
        - 一类特别的函数：只要是同样的输入(实参)，必定得到同样的输出(函数返回值)。
        - 必须遵守一下一些约束：
            - 不得改写参数数据
            - 不会产生任何副作用, 例如网络请求，输入和输出设备
            - 不能调用Date.now()或者Math.random()等不纯的方法
        - **redux的reducer函数必须是一个纯函数**

- redux开发者工具
    - redux DevTools
    - 配置方法：
        - npm install redux-devtools-extension
        - 在store中配置
        ```js
            //引入redux-devtools-extension
            import {composeWithDevTools} from 'redux-devtools-extension'

            const store = createStore(countReducer, composeWithDevTools(applyMiddleware(thunk)))
        ```

- 项目打包运行
    - npm install -g serve
    - serve build

------------
# 案例汇总笔记
## 1. 求和案例_redux精简版
1. 去除Count组件自身的状态
2. src下建立：
    - redux
        - store.js
        - count_reducer.js
3. store.js
    - 引入redux中的createStore函数，创建一个store
    - createStore调用时要传入一个为其服务的reducer
    - 记得暴露store对象
4. count_reducer.js
    - reducer的本质是一个函数，接收：preState, action, 返回加工后的状态
    - reducer有两个作用：
        - 初始化状态
        - 加工状态
    - reducer被第一次调用时，时store自动触发的，
        - 传递的preState是undefined，
        - 传递的action是：{type: '@@REDUX/INIT_a.2.b.4'}
5. 在index.js中监测store中的状态的改变，一旦发生改变重新渲染<App/>
    - *注意*：当前案例使用的是局部监听，只监听了count组件，没有全局监听
    - 备注：redux只负责管理状态，至于状态的改变驱动着页面的展示，要靠自己写。

## 2. 求和案例_redux完整版
- 新增文件：
    - count_action.js  专门用于创建action对象
    - constant.js 放置容易写错的type值

## 3. 求和案例_redux异步action版
- action类型为function时为异步action
    - 明确：延迟的动作不想交给组件自身，想交给action
    - 何时需要异步action: 想要对状态进行操作，但是具体的数据靠异步任务返回
    - 具体编码：
        - npm install redux-thunk, 并配置在store中
        - 创建action的函数不再返回一般对象，而是一个函数，该函数中写异步任务。
        - 异步任务有结果后，分发一个同步的action去真正操作数据。
    - 备注：异步action不是必须要写的，完全可以自己等待异步任务的结果了再去分发同步action

## 4. 求和案例_react-redux基本使用
- 两个概念：
    - UI组件：不能使用任何redux的api，只负责页面的呈现、交互等。
    - 容器组件：负责和redux通信，将结果交给UI组件。
- 创建容器组件--react-redux的connect函数
    - connect(mapStateToProps, mapDispatchToProps)(UI组件)
        - mapStateToProps：映射状态，返回值是一个对象
        - mapDispatch：映射操作状态的方法，返回值是一个对象
- 备注1：容器组件中的store是靠props传进去的，而不是在容器组件中直接引入
- 备注2：mapDispatchToProps也可以是一个对象

## 5. 求和案例_react-redux优化
   - 容器组件和UI组件整合一个文件
   - 无需自己给容器组件传递store，给<App/>包裹一个<Provider store={store}>即可。
   - 使用了react-redux后也不用再自己监测redux中状态的改变了，容器组件自己可以完成这个工作。
   - mapDispatchToProps也可以简单的写成一个对象
   - 一个组件要和redux“打交道”要经过哪几步？
        1. 定义好UI组件
        2. 引入connect生成一个容器组件，并暴露，写法如下：
            ```js
                connect(
                    state => ({key: value}), // 映射状态
                    {key: xxxxxAction} // 映射操作状态的方法
                )(UI组件)
            ```
        3. 在UI组件中通过this.props.xxxxxxx读取和操作状态

## 6. 求和案例_react-redux数据共享版
   - 定义一个Person组件，和Count组件通过redux共享数据。
   - 为Person组件编写：reducer、action, 配置constant常量。
   - ***重点***：Person的reducer和Count的reducer要使用combineReducers进行合并，
        合并后的总状态时一个对象！！！
   - 交给store的是总reducer, 最后注意在组件中取出状态的时候，记得“取到位”。

## 7. 求和案例_react-redux-redux开发者工具使用版
    - redux DevTools
    - 配置方法：
        - npm install redux-devtools-extension
        - 在store中配置
        ```js
            //引入redux-devtools-extension
            import {composeWithDevTools} from 'redux-devtools-extension'

            const store = createStore(countReducer, composeWithDevTools(applyMiddleware(thunk)))
        ```

## 8. 求和案例_react-redux最终版
    - 所有变量名字要规范，尽量触发对象的简写形式，
    - reducers文件夹中，编写index.js专门用于汇总并暴露所有的reducer