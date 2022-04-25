> [ES6文档](https://www.w3cschool.cn/escript6/escript6-odgx37ff.html)

//转码结果输出到标准输出
$ npx babel example.js
//转码结果写入一个文件
// --out-file 或 -o 参数指定输出文件
$ npx babel example.js --out-file compiled.js
//或者
$ npx babel example.js -o compiled.js
//整个目录转码
//--out-dir 或 -d 参数指定输出目录
$ npx babel src --out-dir lib
// 或者
$ npx babel src -d lib
//-s 参数生成source map文件
$ npx babel src -d lib -s

# Module
- import 在静态解析阶段执行
    - 由于 import 是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。
- require 运行时加载

- import()
    - 按需加载
    ```js
    button.addEventListener('click', event => {
      import('./dialogBox.js')
      .then(dialogBox => {
        dialogBox.open();
      })
      .catch(error => {
        /* Error handling */
      })
    });
    ```
    - 条件加载
    ```
    if (condition) {
      import('moduleA').then(...);
    } else {
      import('moduleB').then(...);
    }
    ```
    - 动态的模块路径
    ```
    import(f())
    .then(...);
    ```