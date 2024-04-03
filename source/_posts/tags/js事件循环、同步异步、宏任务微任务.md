### js事件循环、同步异步、宏任务微任务

- ​	**js是单进程的，浏览器是多进程的。**浏览器每一个 tab 标签都代表一个独立的进程，其中浏览器渲染进程（浏览器内核）属于浏览器多进程中的一种，主要负责页面渲染，脚本执行，事件处理等，其包含的线程有：GUI 渲染线程（负责渲染页面，解析 HTML，CSS 构成 DOM 树）、JS 引擎线程、事件触发线程、定时器触发线程、http 请求线程等主要线程。

- **主线程**：也就是 js 引擎执行的线程，这个线程只有一个，页面渲染、函数处理都在这个主线程上执行。
  **工作线程**：也称幕后线程，这个线程可能存在于浏览器或js引擎内，与主线程是分开的，处理文件读取、网络请求等异步事件。

  **任务队列( Event Queue )**

  > 所有的任务可以分为同步任务和异步任务，同步任务，顾名思义，就是立即执行的任务，同步任务一般会直接进入到主线程中执行；而异步任务，就是异步执行的任务，比如ajax网络请求，setTimeout 定时函数等都属于异步任务，异步任务会通过任务队列的机制(先进先出的机制)来进行协调。具体的可以用下面的图来大致说明一下：![img](https://pic4.zhimg.com/80/v2-1337770fcc29d10325ee4eb127496fff_720w.jpg)

- 事件循环的过程（即以上“不断读取”过程的细分）![img](https://pic3.zhimg.com/80/v2-a38ad24f9109e1a4cb7b49cc1b90cafe_720w.jpg)

  

- 宏任务：主要包含：script( 整体代码)、setTimeout、setInterval、I/O、UI 交互事件、setImmediate(Node.js 环境)
- 微任务：Promise、MutaionObserver、process.nextTick(Node.js 环境)
- 同步：从上到下有序执行
- 异步：执行顺序不确定，在各自的执行阶段中发生。如：setTimeout和setInterval、*ajax、事件绑定等*

```javascript
setTimeout(function () {
    new Promise(function (resolve, reject) {
        console.log('异步宏任务promise');
        resolve();
    }).then(function () {
        console.log('异步微任务then')
    })
    console.log('异步宏任务');
}, 0)
new Promise(function (resolve, reject) {
    console.log('同步宏任务promise');
    resolve();
}).then(function () {
    console.log('同步微任务then')
})
console.log('同步宏任务')

setTimeout(() => {
    console.log('(5)异步1任务time1');
    new Promise(function (resolve, reject) {
        console.log('(6)异步1宏任务promise');
        setTimeout(() => {
            console.log('(8)异步1任务time2');
        }, 0);
        resolve();
    }).then(function () {
        console.log('(7)异步1微任务then')
    })
}, 0);

console.log('(1)主线程宏任务');

setTimeout(() => {
    console.log('(9)异步2任务time2');

}, 0);
```

setTimeout是异步任务，虽然他在0秒后执行但仍排在队列的后面，因此其中的代码全部靠后执行；new Promise是同步任务同时也是主任务，因此第一行先打印'同步宏任务promise'，then是微任务所以靠后执行，先执行第16行代码，之后再执行第13行的then语句，因此第二行输出为'同步宏任务'，第三行为'同步微任务then'；接下来执行setTimeout中的语句，then因为是微任务所以在第8行执行完成后再执行。

执行顺序总结：先同步再异步，在此基础上先宏任务再微任务



引用

[一个 web 页面的生命周期](https://juejin.cn/post/6930571801382780941)
[【JS】深入理解事件循环,这一篇就够了!(必看)](https://zhuanlan.zhihu.com/p/87684858)
