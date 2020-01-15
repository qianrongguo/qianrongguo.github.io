this.setState 同步和异步特性：

    setState 只在合成事件和钩子函数中是“异步”的，在原生事件和 setTimeout 中都是同步的。
    setState的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。
    setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次 setState ， setState 的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时 setState 多个不同的值，在更新时会对其进行合并批量更新。
  https://juejin.im/post/5bf1444cf265da614a3a1660
  
写一个闭包：

```js
    function foo() {
        let a = 2;
    
        function bar() {
            console.log( a );
        }
    
        return bar;
    }
    
    let baz = foo();
    
    baz();
   ```

防抖和节流：

    函数防抖和函数节流都是防止某一时间频繁触发，但是这两兄弟之间的原理却不一样。
    函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行。
    
  结合应用场景
    
  - debounce
    search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
    window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
    
    
  - throttle
    鼠标不断点击触发，mousedown(单位时间内只触发一次)
    监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断
    
   https://juejin.im/post/5b8de829f265da43623c4261


什么事真实dom?什么是虚拟dom?


localStorage 和 sessionStorage 的区别？


es6 的新特性：


React怎么判断重新渲染该组件：


js的同源策略？



手写一个冒泡排序算法：



jsde的执行顺序：



