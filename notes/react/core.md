> **基于16.8版本，扩展新特性**

- babel: 用于ES6 ==> ES5; jsx ==> js; [bable文档](https://www.babeljs.cn/docs/)

- 虚拟DOM的两种创建方式
    1. js创建虚拟DOM
    ```js
    const VDOM = React.creatElement('h1', {id: 'title'}, 'Hello, React');
    ```
    2. jsx创建虚拟DOM
    ```js
    const VDOM = <h1>Hello, React</h1>
    ```

+ 虚拟DOM和真实DOM:
    1. 本质是Object类型的对象(一般对象)；
    2. 虚拟DOM比较‘轻’（属性少），真是DOM比较‘重’，因为虚拟DOM是React内部在用，无需真是DOM上那么多属性。
    3. 虚拟DOM最终会被React转化为真实DOM，呈现到页面上。

+ jsx语法规则：
    1. 定义虚拟DOM时，不要写引号；
    2. 标签中混入JS表达式时要用{}；
    3. 可以写表达式但不能写语句；
    3. 样式的类名指定不要用class, 要用className;
    4. 内联样式，要用style={{key:value}}的形式去写；
    5. 只有一个根标签；
    6. 标签必须闭合；
    7. 标签首字母
        + 若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
        + 若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。

+ 模块化
    - 理解：将js功能拆分类别，每一类构成一个js文件，也就是js模块
    - 作用：复用js, 简化js的编写，提高js运行效率
+ 组件化
    - 理解：实现局部功能效果的代码和资源的集合(html/css/js/image等等)
    - 作用：复用编码，简化项目代码，提供运行效率

- 面向组件编程
    1. 函数式组件
    ```js
    // 创建函数式组件
    function MyComponent() {
        return <h2>我是用函数定义的组件（适用于【简单组件】的定义）</h2>
    } 
    ```
    - 类式组件
    ```js
    //1. 创建类式组件
    class MyComponent extends React.Component {
        render() {
            //render是放在哪里的？—— MyComponent的原型对象上，供实例使用。
            //render中的this是谁？—— MyComponent的实例对象。MyComponent组件的实例对象。
            console.log('render中的this:', this);
            return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>
        }
    }
    
    //2.渲染组件到页面
    ReactDOM.render(<MyComponent />, document.getElementById('test'));
    /*
        执行了ReactDOM.render(<MyComponent />...)之后，发生了什么？
        1. React解析组件标签，找到了MyComponent组件。
        2. 发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
        3. 将render返回的虚拟DOM转为真实DOM, 随后呈现在页面中。
    */
    ```

+ 简单组件：无状态(无state) --> 函数式组件适用于简单组件
+ 复杂组件：有状态(有state) --> 类式组件适用于复杂组件

- **state**
    + 理解：
        1. state是组件对象最重要的属性，值是对象(包含多个key-value的组合)
        2. 组件被称为“状态机”，通过更新组件的state来更新对应的页面显示(重新渲染组件)
    + 强烈注意：
        1. 组件中render方法中的this为组件实例对象
        2. 组件自定义的方法中this为undefined, 如何解决？
            - 强制绑定this：通过函数对象的bind()
            - 箭头函数
        3. 状态数据，不能直接修改或更新，需要使用setState方法

- 类中方法中的this指向问题
    - changeWeather放在哪里？—— Weather的原型对象上，供实例使用
    - 由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用
    - 类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
    ```js
    class Weather extends React.Component{
        constructor(props){
            super(props)
            //初始化状态
            this.state = { isHot: false }
        }
        render() {
            //读取状态
            const { isHot } = this.state;
            return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
        }
        changeWeather(){
            //changeWeather放在哪里？—— Weather的原型对象上，供实例使用
            //由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用
            //类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
            console.log(this);
        }
    }
    //2.渲染组件到页面
    ReactDOM.render(<Weather/>, document.getElementById('test'));
    ```
- 解决类中this指向问题
    - bind(): `this.changeWeather = this.changeWeather.bind(this) `
    - 箭头函数：箭头函数中的this指向函数外部

- **props**
    - props是只读属性
    - 定义props类型：static propTypes
    - 给props设置默认值：static defaultProps
    - 构造器与props: 构造器是否接收props，是否传递给super，取决于：是否希望在构造器中通过this访问props

- **ref**
    + 字符串类型的ref: 过时了，不推荐使用；
    + 回调形式的ref：比较常用，内联回调引发的两次回调问题无关紧要，可忽略；
        1. 现象：内联回调函数，当数据更新时，会调用两次内联回调，第一次值为null，第二次为节点值；
        2. 原因：react在更新时，会清掉旧的ref并且设置新的；
        3. 解决：将ref的回调函数定义成实例的绑定函数；
    + createRef：官方推荐使用；

- 受控组件与非受控组件：
    - 受控组件：可输入的dom节点，随着输入，值就进入state中维护，就是受控组件
    - 非受控组件：可输入的节点的值是【现用现取】的

- 双向绑定(Vue中的概念)
    - react通过onChange事件实现双向数据绑定的效果

- 高阶函数：如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数
    - 1.若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。
    - 2.若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。
    - 常见的高阶函数有：Promise、serTimeout、arr.map()等。。。

- 函数柯里化：
    - 通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。

## 生命周期(旧)
    1. 初始化阶段：由ReactDOM.render()触发 --- 初次渲染
        1. constructor()
        2. componentWillMount()
        3. render()
        4. componentDidMount() ---- 【常用】
            - 一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
    2. 更新阶段：由组件内部的this.setState()或父组件render触发
        1. shouldComponentUpdate()
            - 返回值为true，调用render，更新视图
            - 返回值为false，不更新视图
        2. componentWillUpdate()
        3. render() ---- 【常用】
        4. componentDidUpdate()
    3. 卸载组件：由ReactDOM.unmountComponentAtNode()触发
        1. componentWillUnmount()  ---- 【常用】
            - 一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息

    + 挂载流程图见：![2_react生命周期(旧).png](../../img/2_react生命周期(旧).png)

------
> **基于17.0.2版本**

## 生命周期（新）
- 对比新旧生命周期
    新生命周期中，废弃了3个旧生命周期:
    + componentWillMount
    + componeentWillUpdate
    + componentWillReceiveProps
    新增了两个生命周期: 使用场景不多
    + getDerivedStateFromProps
    + getSnapshotBeforeUpdate


- 总结生命周期(新)
    1. 重要的钩子
        + render: 初始化渲染或更新渲染调用
        + componentDidMount: 开启监听，发送ajax请求
        + componentWillUnmount: 做一些收尾工作，如：清理定时器
    2. 即将废弃的钩子：
        + componentWillMount
        + componentWillReceiveProps
        + componentWillUpdate


## DOM的diffing算法
+ 对比的最小粒度是【标签】
+ 对数据逆序等【破坏顺序】的操作时：当一个list节点中包裹输入类DOM节点时，key值使用index会使输入类节点出现错乱
+ 虚拟DOM没有value值，真实DOM才有value值，才能留下用户的输入

1. react/vue中的key有什么作用？（key的内部原理是什么？）
    1. 简单的说：key是虚拟DOM对象的标识，在更新显示时key起着极其重要的
    2. 详细的说：当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】，
                 随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较；
                 比较规则如下：
        + 旧虚拟DOM中找到了与新虚拟DOM中相同的key：
            1. 若虚拟DOM中的内容没变，直接使用之前的真实DOM;
            2. 若虚拟DOM中的内容变了， 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
        + 旧虚拟DOM中未找到与新虚拟DOM相同的key：
                根据数据创建新的真实DOM，随后渲染到页面。

2. 为什么遍历列表时，key最好不要用index?
- 用index作为key可能会引发的问题：
    1. 若对数据进行：逆序添加、逆序删除等破坏顺序的操作：
        会产生没有必要的真实DOM更新 ==> 界面效果没问题，但效率低。
    
    2. 如果结构中还包含输入类的DOM:
            会产生错误的DOM更新 ===> 界面有问题。
    
    3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，
              仅用于渲染列表用于展示，使用index作为key是没有问题的。


## 组件化编码流程
1. 拆分组件
2. 实现静态组件
3. 实现动态组件
    1. 初始化数据显示
        - 数据类型
        - 数据名称
        - 保存在哪个组件？
    2. 交互(从绑定事件监听开始)

### 总结todoList案例相关知识点：
1. 拆分组件、实现静态组件，注意：className、style的写法
2. 动态初始化列表，如何确定将数据放在哪个组件的state中？
    - 某个组件使用： 放在其自身的state中
    - 某些组件使用： 放在他们共同的父组件state中（官方称此操作为：状态提升）
3. 关于父子之间通信：
    + 【父组件】给【子组件】传递数据：通过props传递；
    + 【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数；
4. 注意defaultChecked和checked的区别，类似的还有：defaultValue和value;
    + defaultChecked只在组件第一次渲染时起作用
5. 状态在哪里，操作状态的方法就在哪里

## 前端配置代理（react脚手架）
   - 方法一：
       - 在package.json中追加如下配置
        ```js
        "proxy":"http://localhost:5000"
        ```
    - 方法二：
        在src下创建配置文件：src/setupProxy.js

### github搜索案例相关知识点
1. 设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。
2. ES6小知识点：解构赋值 + 重命名
    ```js
    let obj = {a: {b: 1}}
    const {a} = obj; // 传统解构赋值
    const {a: {b}} = obj; // 连续解构赋值
    const {a: {b: value}} = obj; // 连续解构赋值 + 重命名
    ```
3. 消息订阅与发布机制
    1. 先订阅，再发布（理解：有一种隔空对话的感觉）
    2. 适用于任意组件间通信
    3. 要在组件的componentWillUnmount中取消订阅
4. fetch发送请求（关注分离的设计思想）
    ```js
    try{
        const response = await fetch(`/api1/search/users2?q=${keyWord}`);
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.log('请求出错', error);
    }
    ```

## 前端路由原理
1. BOM的history对象
    - History.createBrowserHistory() // H5的history
2. hash值（锚点#）
    - History.createHashHistory() // hash值

- BrowserRouter和HashRouter的区别
1. 底层原理不一样：
    - BrowserRouter使用的是H5的history API，不兼容IE9及一下版本。
    - HashRouter使用的是URL的哈希值。
2. url表现形式不一样
    - BrowserRouter的路径中没有#，例如：localhost:3000/demo/test
    - HashRouter的路径包含#，例如：localhost:3000/#/demo/test
3. 刷新后对路由state参数的影响
    - BrowserRouter没有任何影响，因为state保存在history对象中。
    - HashRouter刷新后会导致路由state参数的丢失。
4. 备注：HashRouter可以用于解决一些路径错误相关的问题。