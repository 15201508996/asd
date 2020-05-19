# JS实现Promise原理

---

 promise是用来解决Js中的异步问题，js中所有的异步可从callback->promise->generator+co=async+wait，其实所有的都是callback的变相处理，只是后者慢慢变得越来越优雅和强壮可扩展。

那么如何实现promise呢？先观察一下promise的样子

```
let a = new Promise((resole,reject)=>{
  // dosomething
  resole()
})
```

无非是一个名称叫Promise的对象，然后传参一个函数，（resolve， reject）=> { resolve()},promise的强大之处在于回调处理，promise.then(resoleFuntion, rejectFunction),第一个函数是resolve的回调处理函数，第二个事reject说到resolve，reject，那么还得再提一下pending，promise对象内部有以上三种状态。默认是pending状态
，状态的转变需要我们手动自己去掉函数resolve()或者reject（）
整理一下思路： new promise的时候callback作为传参参数执行promise函数，callback在promise函数内部执行。callback里手动调用的resolve或者reject方法，那么promise对象里肯定是有这两种方法的。
最关键的记住状态，然后再then的时候根据状态去判断到底是执行resolveFunction还是rejectFunction。

```

function PromiseFn(callBack){
    let self = this;
    self.resolveVal = undefined;
    self.rejectVal = undefined;
    self.status = "pending" //默认是等待状态

    function resolve(val){
        if(self.status === 'pending'){
            self.resolveVal  = val;
            self.status = 'resolve'
        }
    }
    function reject(val){
        if(self.status === 'pending'){
            self.rejectVal = val;
            self.status = 'reject'
        }
    }
    //
    try {
          callBack(resolve,reject)
    }catch(e){
         reject();
    }
}

```

把它当做函数fn就行，fn再promise内部执行，然后fn内部刚好有一个resolve（）。
然后再去调用promise的这个resolve执行。
也就是这个原因，座椅callBack的参数必须是resolve和reject。

```
let a = new Promise((resolve,reject)=>{
    console.log('111')
}))
console.log("456")

// 输出
111
456
```

上面我们定义了resolveVal和rejectVal，因为待会在promise调用then执行的时候，会作为参数给resolveFunction或者rejectFunction
所以promise对象内部必须记住这个参数。
下面一起看一下promise.then，promise.then(resolveFunction,rejectFunction).
在promise的原型链上有个then方法。

 ```

PromiseFn.prototype.then = function(resolveFunction,rejectFunction){
    let self = this;
    if(self.status === 'resolve'){
        resolveFunction(self.resolveVal)
    }
    if(self.status === 'reject'){
        rejectFunction(self.rejectVal)
    }
}

 ```

在then执行的时候，去判断内部是什么状态，然后执行对象的resolve或者reject回调函数。

----

```

let promiseA =  new PromiseFn(resolve,reject)=>{
    setTimeout(function(){
       resolve();
    },2000)
}

```

异步的情况下，上面的写法是无法满足这种条件的，当去执行promise.then的时候，发现状态还是pending，不做任何执行。那我们怎么去做了，发现状态是pending，肯定是两秒之内啦，不放先把promise.then两个函数参数先在promise里面记下来，两秒后状态变为resolve了，在执行函数。

 ```
 function PromiseFn(callBack){
     try{
         callBack(resolve, reject)
     }catch(){
         reject()
     }
     let self = this;
     self.resolveVal = undefined;
     self.rrejectVal =  undefined;
     self.status = 'pending'
     self.keepResolveFn = [];
     self.keepRejectFn = []

     function resolve(val){
         if(self.status === 'pending){
             self.resolveVal = val;
             self.status = 'resolve';
             self.keepResolveFn.forEach(fn>fn());
         }
     }
     function reject(val){
         if(self.status === 'pending'){
             self.rejectVal = val;
             self.status = 'reject'
             self.keepRejectFn.forEach(fn=>fn());
         }
     }
     //执行先记录resolve和reject函数事件
 }

 PromiseFn.prototyoe.then = function(resolveFunction,rejectFunction)
{
    let self = this;
    if(self.status === 'resolve'){
        resolveFunction(self,resolveVal)
    }
    if(self.status === 'reject'){
        rejectFunction(self.rejectVal)
    }
    if(self.status === 'pending'){
        self.keepResolveFn.push(()=>{
            resolveFunction(self.resolveVal);
        })
        self.keepRejectFn.push(()=>{
            rejectFunction(self.rejectVal)
        })
    }
}

 ```

所有的then方法执行，也是通过return一个新的peomise对象来执行的，这个才有链式回调。
new Promise().then().then().then();

```

var b = 10;
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        b += 10;
        resolve();
    },1000)
}).then(function(){
    console.log(b);
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            b *= 2;
            resolve();
        }, 1000)
    })
}).then(function(){
    console.log(b);
    return new Promise(resolve,reject)=>{
        setTimeout(()=>{
            b *= b;
            resolve();
        }, 1000)
    }
}).then(function(){
    console.log(b);
})

```




