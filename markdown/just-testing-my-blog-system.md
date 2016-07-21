##just-testing-my-blog-system
[Promise](http://liubin.org/promises-book/#ch2-promise-all)

2.9. Promise.race
接着我们来看看和 Promise.all 类似的对多个promise对象进行处理的 Promise.race 方法。

它的使用方法和Promise.all一样，接收一个promise对象数组为参数。

Promise.all 在接收到的所有的对象promise都变为 FulFilled 或者 Rejected 状态之后才会继续进行后面的处理， 与之相对的是 Promise.race 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理。

像Promise.all时的例子一样，我们来看一个带计时器的 Promise.race 的使用例子。

promise-race-timer.js
// `delay`毫秒后执行resolve
function timerPromisefy(delay) {
    return new Promise(function (resolve) {
        setTimeout(function () {
            resolve(delay);
        }, delay);
    });
}
// 任何一个promise变为resolve或reject 的话程序就停止运行
Promise.race([
    timerPromisefy(1),
    timerPromisefy(32),
    timerPromisefy(64),
    timerPromisefy(128)
]).then(function (value) {
    console.log(value);    // => 1
});
运行
上面的代码创建了4个promise对象，这些promise对象会分别在1ms，32ms，64ms和128ms后变为确定状态，即FulFilled，并且在第一个变为确定状态的1ms后， .then 注册的回调函数就会被调用，这时候确定状态的promise对象会调用 resolve(1) 因此传递给 value 的值也是1，控制台上会打印出1来。

下面我们再来看看在第一个promise对象变为确定（FulFilled）状态后，它之后的promise对象是否还在继续运行。

promise-race-other.js
var winnerPromise = new Promise(function (resolve) {
        setTimeout(function () {
            console.log('this is winner');
            resolve('this is winner');
        }, 4);
    });
var loserPromise = new Promise(function (resolve) {
        setTimeout(function () {
            console.log('this is loser');
            resolve('this is loser');
        }, 1000);
    });
// 第一个promise变为resolve后程序停止
Promise.race([winnerPromise, loserPromise]).then(function (value) {
    console.log(value);    // => 'this is winner'
});
运行
我们在前面代码的基础上增加了 console.log 用来输出调试信息。

执行上面代码的话，我们会看到 winnter和loser promise对象的 setTimeout 方法都会执行完毕， console.log 也会分别输出它们的信息。

也就是说， Promise.race 在第一个promise对象变为Fulfilled之后，并不会取消其他promise对象的执行。
