>  ### [前端基础-JavaScript面试题汇总](https://www.jianshu.com/p/7e71c123b2eb)
 
## 基本数据类型
[01_数据类型.html](./01_%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B.html)
简单数据类型：
+ number
+ string
    - 模板字面量
    ```js
    // 字符串插值, ${}
    let b = 'hahah';
    let a = `<div>${b}1234</div>`
    ```
+ boolean
+ null
+ undefined
+ symbol: 唯一、不可变的; symbol函数不可以和new关键字一起作为构造函数使用。
复杂数据类型
+ object
    - contructor: 用于创建当前对象的函数。
    - hasOwnProperty(propertyName): 用于判断当前对象实例(不是原型)上是否存在给定的属性。要检查的
        属性名必须是字符串(如: o.hasOwnProperty("name"))或符号。
    - isPropertyOf(object): 用于判断当前对象是否为另一个对象的原型。
    - propertyIsEnumerable(propertyName): 用于判断给定的属性是否可以使用for-in语句枚举。

## 基本数据类型和引用数据类型的区别
[JavaScript中基本数据类型和引用数据类型的区别](https://www.cnblogs.com/cxying93/p/6106469.html)

## 堆内存和栈内存的区别
1. 栈内存存储简单数据类型，堆内存存储复杂数据类型
2. 栈内存空间小，读写速度快

## 基本数据类型
+ 原始类型
    - 数值
    - 字符串
    - 布尔
    - symbol(符号)
    - null
    - undefined
+ 对象类型
## 创建对象的几种方式
[创建对象的几种方式](./02_%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F.html)
1. 工厂模式
```js
function createPerson(name, age, job) {
    let o = new Object()
    o.name = name,
    o.age = age,
    o.job = job,
    o.sayName = function() {
        console.log(this.name)
    }
}
let person3 = createPerson('jeck', 28, '主管')
```
2. 构造函数模式
```js
// Person是构造函数
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function() {
        console.log(this.name)
    }
}
// person2是构造函数Person的实例
let person2 = new Person('anni', 20, '老师')
```
3. 原型模式
    ```js
    function Person() {}
    Person.prototype.name = 'nike';
    Person.prototype.age = '19';
    Person.prototype.job = '工程师';
    Person.prototype.sayName = function() {console.log(this.name)}
    let person1 = new Person() {}
    ```  
    - 原型：是原型对象的简称，当创建一个函数A的时候，js会按照特定的规则为这个函数创建一个属性，这个属性就是原型对象(prototype),
            原型对象上默认会有一个属性constructor指向这个函数A,通过在原型对象上添加方法，所有实例都会使用这个方法。
    - hasOwnProperty("xxx"): 用来检查实例上是否有xxx属性。
    - 当使用以下格式重写prototype对象的值时，如需要constructor还指向构造函数，则需重写时加上construtor属性；
      ```js
      function Person() {}
      Person.prototype = {
          constructor: Person, // 这时的constructor属性的[[Enumerable]]为true
          name: 'nick',
          ...
      }
      ```
    - 在重写构造函数上的原型之后再创建的实例才会引用新的原型。而在此之前创建的实例仍然会引用最初的原型。
      ```js
        function Person() {}
        let person1 = new Person();
        Person.prototype = {
            name: 'nike'
        }

        let person2 = new Person();

        console.log(person1.name)  // undefined
        console.log(person2.name) // nike
      ```
    - 原型的问题：
        1. 弱化了向构造函数传递初始化参数的能力，会导致所有实例默认都取得相同的属性值；
        2. **真正的问题来自包含引用值的属性：因为原型的共享特性，当实例修改原型上的引用属性时，
        会导致其他实例上的该引用属性同时变化，这是因为引用属性都指向同一个地址。

## 变量
### let, const, var
1. let和const是块级作用域，var是函数级作用域
2. let和const没有声明提升，var有声明提升
3. let和const在全局作用域中声明的变量不会成为window对象的属性，但是var会
4. const声明的是个常量，如果声明的变量类型是对象，则const声明的限制只适用于它指向的变量的引用，对象内部属性变化不受限制


## 深浅拷贝
[浅拷贝和深拷贝](https://www.jianshu.com/p/3778af0dbb88)
### 理解：
浅拷贝：仅仅是复制了引用(地址)，相互之间会影响。
深拷贝：在堆中重新分配了内存空间，不同地址，相同的值，不会互相影响。
### 浅拷贝的方法：
1. Object.values()
```js
Object.values({name: 'jamie', age: 18}), // ['jamie', 18]
```
2. Object.entries()
```js
Object.entries({name: 'jamie', age: 18}), // [['name', 'jamie'], ['age', '18']]
```

### 深拷贝的方法：
1. 
2. 

## 作用域链

## 闭包

## this指向

## call,applay,bind
### 共同点：
改变this指向，第一个参数是谁，this就指向谁
### 不同点：
+ call：返回函数调用的返回值；第二个参数和第N个参数逗号分隔
+ applay：返回函数调用的返回值；第二个参数是一个数组
+ bind：返回一个函数的定义；第二个参数和第N个参数逗号分隔; bind只生效一次
```js
var name = '小王', age=17;
var obj = {
    name: '小张',
    objAge: this.age,
    myFun: function(fm, t) {
        console.log(this.name + ' 年龄 ' + this.age,' 来自 ' + fm + '去往' + t);
    }

var db = {
    name: '德玛',
    age: 99

obj.myFun.call(db,'成都','上海');　　　　 // 德玛 年龄 99  来自 成都去往上海
obj.myFun.apply(db,['成都','上海']);      // 德玛 年龄 99  来自 成都去往上海  
obj.myFun.bind(db,'成都','上海')();       // 德玛 年龄 99  来自 成都去往上海
obj.myFun.bind(db,['成都','上海'])();　　 // 德玛 年龄 99  来自 成都, 上海去往 undefined
obj.myFun.bind(this, '佳木斯','南京')();  // 小王 年龄 17  来自 佳木斯去往南京
```

### **bind:**
```js
function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var h = g.bind({a:'yoo'}); // bind只生效一次！
console.log(h()); // azerty

var o = {a:37, f:f, g:g, h:h};
console.log(o.a, o.f(), o.g(), o.h()); // 37, 37, azerty, azerty
```

**总结**：call和apply都是改变上下文中的this并立即执行这个函数，bind方法可以让对应的函数想什么时候调用就什么时候调用，并且可以将参数在执行的时候添加，这就是它们的区别。

## 原型链
[原型链](./03%E7%BB%A7%E6%89%BF/01_原型链和继承.html)
> **任何函数的默认原型都是一个Object的实例, 实例的内部指针指向Object.prototype**
> **需要注意的一点是：通过对象字面量给原型添加新方法会重置原型**
```js
// 这种给原型添加方法的方式会让原型重置为只有这个getValues方法
SubType.prototy = {
    getValues = function() {}
}
```
```js
[[Prototype]] // 是实例的内部指针
```
> 原型链的问题
1. 引用类型共享属性
2. 子类型在实例化时不能给父类型的构造函数传参

## 继承
1. 原型链继承;
    + 核心：将父类的实例作为子类的原型
    + 缺点：
        1. 原型中包含的引用值会在所有实例间共享，若某一实例修改了原型中的引用值，其结果会在所有实例上反映
        2. 子类型在实例化时不能给父类型的构造函数传参
    ```js
    function SuperType() {
        this.property = true;
    }
    SuperType.prototype.getSuperValue = function () {
        return this.property;
    }
    function SubType() {
        this.subproperty = false;
    }
    // 继承SuperType, 重写了SubType最初的原型
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function() {
        return this.subproperty;
    }
    let instance = new SubType()
    console.log(instance.getSuperValue()) // true
    ```
2. 借用构造函数（经典继承）;
    + 核心：在子类的内部调用父类，通过call改变父类中this的指向。**等于是复制父类的实例属性给子类**
    + 特点： 相比于原型链，借用构造函数的一个优点就是可以在子类构造函数中向父类构造函数传参。
    + 缺点：
        1. 实例只是子类的实例，并不是父类的实例
        2. 无法继承父类原型中的方法
        3. 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能
    ```js
    function SuperType(name) {
        this.name = name;
    }
    SuperType.prototype.sayName = function() {console.log(this.name)}
    function SubType() {
        SuperType.call(this, "Nick") // 继承SuperType并传参
        this.age = 29; // 实例属性
    }
    SubType.prototype.subAge = function() {console.log(this.age)}
    let instance = new SubType();
    console.log(instance.name) // "Nick"
    console.log(instance.age) // 29
    console.log(instance.sayName()) // 报错：无法继承原型中的方法
    ```
3. 组合继承（原型链继承 + 借用构造函数继承）;
    + 核心：结合了两种模式的优点，传参和复用
    + 特点：
        1. 可以继承父类原型上的属性，可以传参，可复用。
        2. 每个新实例引入的构造函数属性是私有的。
    + 缺点：调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。
    ```js
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    }
    SuperType.prototype.sayName = function() {console.log(this.name)};
    function SubType(name, age) {
        SuperType.call(this, name); // 继承属性
        this.age = age;
    }
    //继承方法
    SubType.prototype = new SuperType();
    SubType.prototype.sayAge = function() {console.log(this.age)};

    let instance1 = new SubType('Nick', 29);
    instance1.colors.push('black');
    console.log(instance1.colors); // ['red', 'blue', 'green', 'black']
    instance1.sayName(); // Nick
    instance1.sayAge(); // 29

    let instance2 = new SubType('Greg', 27);
    console.log(instance2.colors); // ['red', 'blue', 'green']
    instance2.sayName(); // Greg
    instance2.sayAge(); // 27
    ```
4. 原型式继承;
    + 核心：用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。**object.create()**就是这个原理。
    + 特点：类似于复制一个对象，用函数来包装。
    + 缺点：
        1. 所有实例都会继承原型上的属性。
        2. 无法实现复用。(实例属性都是后面添加的)
    ```js
    function object(o) {
        function F() {}
        F.prototype = o;
        return new F();
    }
    let person = {
        name: 'Nick',
        friends: ['Shelby', 'Court', 'Van']
    }
    let anotherPerson = object(person);
    anotherPerson.name = 'Greg';
    anotherPerson.friends.push('Rob');
    let yetAnotherPerson = object(person);
    yetAnotherPerson.name = 'Linda';
    yetAnotherPerson.friends.push('Barbie');
    console.log(person.friends) // ['Shelby', 'Court', 'Van', 'Rob', 'Barbie']
    ```
5. 寄生式继承;
    + 核心：就是给原型式继承外面套了个壳子。
    + 特点：没有创建自定义类型，因为只是套了个壳子返回对象(这个)，这个函数顺理成章就成了创建的新对象
    + 缺点：没用到原型，无法复用。
    ```js
    function object(o) {
        function F() {}
        F.prototype = o;
        return new F();
    }
    function createAnother(original) {
        let clone = object(original);
        clone.sayHi = function() {console.log('hi');}
        return clone;
    }
    let person = {
        name: 'Nick',
        friends: ['Shelby', 'Court', 'Van']
    }
    let anotherPerson = createAnother(person);
    anotherPerson.sayHi();
    ```
6. _寄生式组合继承_(引用类型继承的最佳模式);
    + 核心：修复了组合继承的问题
    + 特点：引用类型继承的最佳模式
    ```js
    function object(o) {
        function F() {}
        F.prototype = o;
        return new F();
    }
    function inheritPrototype(subType, superType) {
        let prototype = object(superType.prototype); // 创建对象
        prototype.constructor = subType;  // 增强对象
        subType.prototype = prototype;  // 赋值对象
    }
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    }
    SuperType.prototype.sayName = function() { console.log(this.name) }
    function SubType(name, age) {
        SuperType.call(this, name);
        this.age = age;
    }
    inheritPrototype(SubType, SuperType);
    SubType.prototype.sayAge = function() {
        console.log(this.age)
    }
    ```
> 参考文章：[js继承【面试题】](https://blog.csdn.net/qq_45846359/article/details/108364725)

## new做了什么
1. 在内存中创建了一个新对象
2. 这个新对象内部的[[Prototype]]指针被赋值为构造函数的prototype属性
3. 构造函数内部的this指向这个新对象。
4. 执行构造函数内部的代码（新对象拥有构造函数内部的属性）
5. 如果构造函数返回非空对象，则返回该对象；否则，返回刚创建的新对象。

## js如何实现一个类
类是一种特殊的函数，使用typeof操作符检测类标识符，结果是个函数：
```js
class Person {}
console.log(typeof (Person)) // function
```
### 类和函数的区别
+ 声明提升：函数声明可以提升，但是类定义不能；
+ 作用域：函数受函数作用域限制，而类受块作用域限制；
    ```js
    {
        function FunctionDeclaration() {}
        class ClassDeclaration {}
    }
    console.log(FunctionDeclaration); // FunctionDeclaration() {}
    console.log(ClassDeclaration); // ReferenceError: ClassDeclaration is not defined
    ```
+ 类定义的代码都是在严格模式下执行的

### 类构造函数和构造函数
**最主要的区别是**：调用类构造函数必须使用new操作符，而普通构造函数如果不使用new调用，那么就会以全局的this作为内部对象。调用类构造函数如果忘了使用new则会抛出错误：
```js
function Person() {}
class Animal {}
// 把window作为this来构建实例
let p = Person();
let a = Animal(); // TypeError: class constructor Animal cannot be invoked without 'new'
```


## 什么是面向对象？
**面向对象是一种编程思想**，JS本身就是基于面向对象构建出来的（例如：JS中有很多内置类，像Promise就是ES6中新增的一个内置类，我们可以基于new Promise来创建一个实例，来管理异步编程，我在项目中，Promise也经常用，自己也研究过它的源码...）, 我们平时用的VUE/REACT/JQUERY也是基于面向对象构建出来的，他们都是类，平时开发的时候都是创建他们的实例来操作的；当然我自己在真实项目中，也封装过一些组件插件（例如：DIALOG、拖拽、....），也是基于面向对象开发的，这样可以创造不同的实例，来管理私有的属性和公有的方法，很方便...
**Js中的面向对象**，和其他编程语言还是有略微不同的，JS中类和实例是基于原型和原型链机制来处理的；而且JS中关于类的重载、重写、继承也和其他语言不太一样...
 
## 面向过程和面向对象的区别？
+ **面向过程**
    1. 理解：**面向过程**的思想是把一个项目、一件事情按照一定的顺序，从头到尾一步一步地做下去，先做什么，后做什么，一直到结束。这种思想比较好理解，其实这也是一个人做事的方法。
    2. 优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源；
    3. 缺点：没有面向对象易维护、易复用、易扩展。
+ **面向对象**
    1. 理解：**面向对象**的思想是把一个项目、一件事情分成更小的项目，或者说分成一个个更小的部分，每一个部分负责什么方面的功能，最后再由这些部分组合而成为一个整体。这种思想比较适合多人的分工合作，就像一个大的机关，分成各个部门，每个部门负责某样职能。
    2. 优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护。
    3. 缺点：性能比面向过程低。

## instanceof 操作符
可以检查这个对象是不是类/构造函数的实例

## 事件队列

## 宏任务与微任务

## js事件传播机制

## 事件代理

## 阻止事件冒泡

## 