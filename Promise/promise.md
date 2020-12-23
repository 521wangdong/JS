# Promise

## 基础

```js
const promise = new Promise((resolve,reject) => {
        console.log(1)
        resolve()
        console.log(2)
    })

    promise.then(() => {
        console.log(3)
    })

    console.log(4)

    //1,2,4,3
```

Promise 构造函数是同步执行的，promise.then 中的函数是异步执行的


```js
const one = () =>(new Promise((resolve,reject) => {

        console.log(1)

        let two = new Promise((resolve,reject) => {
            console.log(2)
            setTimeout(() => {
                console.log(3)
                resolve(4)
            },0)
            resolve(5)
        })
        resolve(2)
        two.then((arg) => {
            console.log(arg)
        })

        
    }))

    one().then((arg) => {
            console.log(arg)
    })

    console.log(6)


        //1,2,6,5,2,3
```


先执行宏任务发现setTimeout，将回调函数放入下一轮任务队列two的then，暂且命名为then1，放入微任务队列，且one也有then，命名为then2，放入微任务队列。执行console.log(6),输出 6，宏任务执行结束
再执行微任务，执行then1,执行then2
第一轮事件循环结束，开始执行第二轮。第二轮事件循环先执行宏任务里面的，也就是setTimeout的回调，输出 3.resolve(4)不会生效，因为p的Promise状态一旦改变就不会再变化了。



## Event Loop

js是单线程的
单线程任务分为同步任务和异步任务
异步任务又分为宏任务（MacroTask）和微任务（MicroTask）

Event Loop即事件循环，是指浏览器或Node的一种解决javaScript单线程运行时不会阻塞的一种机制，也就是我们经常使用异步的原理。

从script开始，任务按顺序进入执行栈中，同步任务在主线程中直接被执行，遇到异步任务时，异步任务会进入异步处理模块并注册相应的回调函数，注册完成后回调函数进入任务队列（遇到微任务进入微任务队列，遇到宏任务开启一个新的宏任务队列），待同步任务执行完执行栈为空时，依次进入栈中执行，每次执行宏任务之前会检查是否存在微任务，如果存在则将微任务队列中的所有任务执行完后再执行宏任务，如此循环

每遇到一个宏任务会开启一个新的宏任务队列，而微任务队列只有一个，执行宏任务若发现新的微任务，会将微任务加入微任务队列，所以当开始执行一个新的宏任务时，如果微任务队列中有新的任务，先执行一遍微任务


## 同步任务和异步任务

Javascript单线程任务被分为同步任务和异步任务，同步任务会在调用栈中按照顺序等待主线程依次执行，异步任务会在异步任务有了结果后，将注册的回调函数放入任务队列中等待主线程空闲的时候（调用栈被清空），被读取到栈内等待主线程的执行


### 宏任务

script(全局任务)
setTimeout
setInterval
setImmediate
I/O
UI render

### 微任务

Promise.then()/catch()/finally()
MutationObserver
process.nextTick(node.js)

### 区别

发起	        node/浏览器	 js引擎
运行先后	     后	         先
触发新一轮Tick	 会	         不会


栗子

```js
console.log(1)

setTimeout(() => {
    console.log(2)
    Promise.resolve(3).then(res => console.log(res))
},0)

setTimeout(() => {
    console.log(4)
},0)

Promise.resolve(5).then(res => console.log(res))

console.log(6)

   //1,6,5,2,3,4
```

正常的逻辑同步=>微任务=>宏任务



### Process.nextTick()

如果存在 nextTick 队列，就会清空队列中的所有回调函数，并且优先于其他 microtask 执行。






### 栈（Stack）

栈在计算机科学中是限定仅在表尾进行插入或删除操作的线性表。 栈是一种数据结构，它按照后进先出的原则存储数据，先进入的数据被压入栈底，最后的数据在栈顶，需要读数据的时候从栈顶开始弹出数据。

### 队列（Queue）

队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。 队列中没有元素时，称为空队列。队列的数据元素又称为队列元素。在队列中插入一个队列元素称为入队，从队列中删除一个队列元素称为出队。因为队列只允许在一端插入，在另一端删除，所以只有最早进入队列的元素才能最先从队列中删除，故队列又称为先进先出










