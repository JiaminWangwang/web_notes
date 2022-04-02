1. setState
```js
// 对象式setState
const {count} = this.state
this.setState({count: count + 1}, ()=>{
    console.log(this.state.count)
}
// 函数式setState
this.setState((state, props) => {
    return {count: state.count + 1}
}, ()=> {
    console.log(this.state.count)
})
```
2. lazyLoad
```js
import {lazy, Suspense} from 'react'
import Loading from './Loading'
// 通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包
const Home = lazy(()=> import('./Home'))
const About = lazy(()=> import('./About'))

// 通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
<Suspense fallback={<Loading/>}>
    <Route path="/about" component={About} />
    <Route path="/home" component={Home} />
</Suspense>
```

3. Hooks
    - State Hook
    - Effect Hook
        - React.useEffect() 用于模拟生命周期钩子，可以在函数组件中执行副作用操作
        - React中的副作用操作：
            - 发送ajax请求数据
            - 设置订阅/启动定时器
            - 手动更改真实DOM
        - 语法和说明
            ```js
                useEffect(() => {
                    // 在此可以执行任何带副作用操作
                    return () => { // 在组件卸载前执行
                        // 在此做一些收尾工作，比如清除定时器/取消订阅等
                    }
                }, [stateValue]) // 如果不指定这个useEffect的第二个参数，会监听所有，如果指定参数为[], 则什么都不监听，若数组中有值，则监听该值
            ```
        - 可以把useEffect Hook 看作是如下三个函数的组合
            - componentDidMount()
            - componentDidUpdate()
            - componentWillUnmount()
    - Ref Hook
        - React.useRef() 可以在函数组件中存储/查找组件内的标签或任意其他数据
        - 语法：
            ```js
                const refContainer = React.useRef()
            ```
            ```html
                <input ref={refContainer}>
            ```
        - 作用：保持标签对象，功能与React.createRef()一样

4. Fragment
    - 当不想使用一个无意义的<div>外层标签时，可以使用<Fragment>代替，
        <Fragment>标签在文档加载时会当成没有这个标签处理
    - <Fragment>和<></>有同样的效果
        - <Fragment key={1}> 可以有一个属性key
        - <></> 空标签不可以有任何属性

5. Context
    - 理解：一种组件间通信方式，常用于【祖组件】与【后代组件】间通信
    - 使用
        1. 创建Context容器对象：
            ```js
            const XxxContext = React.createContext()
            ```
        2. 渲染子组件时，外面包裹XxxContext.Provider, 通过value属性给后代组件传递数据：
            <XxxContext.Provider value={数据}>
                子组件
            </XxxContext.Provider>
        3. 后代组件读取数据：
            ```js
            // 第一种方式：仅适用于类组件
            static contextType = XxxContext // 声明接收context
            this.context // 读取context中的value数据

            // 第二种方式：函数组件与类组件都可以
            <XxxContext.Consumer>
                {
                    value => { // value就是context中的value数据
                        return (
                            要显示的内容
                        )
                    }
                }
            </XxxContext.Consumer>
            ```
    - **注意**
        在应用开发中一般不用context，一般都用它的封装react插件

6. PureComponent
    - Component的2个问题：
        - 只要执行setState(), 即使不改变状态数据，组件也会重新render() ===> 效率低
        - 当父组件状态改变重新render()时，子组件即使没有用到父组件的数据，子组件也依然会重新render() ===> 效率低
    - 效率高的做法：
        只有当组件的state或props数据发生改变时才冲洗你render()
    - 原因：
        Component中的shouldComponentUpdate()总是返回true
    - 解决：
        - 方法1：
            - 重写shouldComponentUpdate()方法
            - 比较新旧state或props数据，如果有变化才返回true, 如果没有变化返回false
        - 方法2：
            - 使用PureComponent
            - PureComponent重写了shouldComponentUpdate(), 只有state或props数据有变化才返回true
            - 注意：
                - 只是进行state和props数据的浅比较，如果只是数据对象内部数据变了，返回false
                - 不要直接修改state数据，而是要产生新数据
            - 项目中一般使用PureComponent来优化

7. render props
    - 如何向组件内部动态传入带内容的结构(标签)？
        - Vue中：
            - 使用slot技术，也就是通过组件标签体传入结构：
                ```js
                <A>
                    <B/>
                </A>
                ```
        - React中：
            - 使用children props: 通过组件标签体传入结构
                ```js
                <A>
                    <B>xxxx</B>
                </A>
                {this.props.children}
                ```
            - 使用render props：通过组件标签属性传入结构，而且可以携带数据，一般用render函数属性
                ```js
                <A render={(data) => <C data={data}></C>} ></A>
                // A组件：{this.props.render(内部state数据)}
                // C组件：读取A组件传入的数据显示 {this.props.data}
                ```

8. ErrorBoundary(错误边界)
    - 理解：
        错误边界：用来捕获**后代**组件错误，渲染出备用页面
    - 特点：
        只能捕获后代组件**生命周期**产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误
    - 使用方式：
        - getDerivedStateFromError配合componentDidCatch
        ```js
        // 生命周期函数，一旦后代组件报错，就会触发
        static getDerivedStateFromError(error) {
            console.log(error);
            // 在render之前触发
            // 返回新的state
            return {
                hasError: true
            }
        }

        componentDidCatch(error, info) {
            // 统计页面的错误，发送请求到后台去
            console.log(error, inso)
        }
        ```

9. 组件间通信方式总结
    - 通信方式：
        - props:
            - children props
            - render props
        - 消息订阅-发布
            - PubSubJS、event等
        - 集中式管理：
            - redux、dva等等
        - conText:
            - 生产者-消费者模式
    - 常见搭配方式：
        - 父子组件：props
        - 兄弟组件: 消息订阅-发布、集中式管理
        - 祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)
