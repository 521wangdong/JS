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

js执行机制

先执行宏任务发现setTimeout，将回调函数放入下一轮任务队列two的then，暂且命名为then1，放入微任务队列，且one也有then，命名为then2，放入微任务队列。执行console.log(6),输出 6，宏任务执行结束
再执行微任务，执行then1,执行then2
第一轮事件循环结束，开始执行第二轮。第二轮事件循环先执行宏任务里面的，也就是setTimeout的回调，输出 3.resolve(4)不会生效，因为p的Promise状态一旦改变就不会再变化了。


