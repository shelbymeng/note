## Promise 对象

### Promise 含义

promise 简单来说是一个容器，保存着未来才会结束的事情（通常为异步操作）。语法上来说是一个对象，可以获取到异步操作的消息。  
promise 对象有以下的特点：

- 对象状态不受外界的影响，promise 对象代表着一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）、reject（已失败）。只有异步操作的结果才可以决定当前的状态，其他操作无法改变。
- 状态改变，就不会再变，任何时候都可以得到这个结果，promise 对象状态改变只有两种可能，从 pending 变为 fulfilled 和从 pending 变为 rejected。只要这两种情况发生，状态凝固。此时称为 resolve。  
  Promise 缺点：

1. 无法取消，一旦新建就会立即执行，无法中途取消。
2. 若没有设置回调函数，promise 内部抛出的错误，不会反映到外部。
3. 当处于 pending 状态，无法得知目前进展到哪一个阶段。

### 用法

Promise 对象是一个构造函数，用来生成 Promise 实例。  
Promis 构造函数接受一个函数作为参数，该函数的两个参数 resolve 和 reject。resolve 是将 Promise 对象的状态从未完成到成功，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject 函数的作用是将 Promise 对象的状态从未完成变为失败，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。  
promise 实例生成后，可以用 then 方法分别制定 resolved 状态和 rejected 状态的回调函数。  
then 方法接受两个回调函数作为参数，第一个参数为成功时的回调函数，第二个参数是失败时回调，第二个参数可选，两个函数都接受 promise 对象传出的值作为参数。

### Promise.prototype.then()

promise 实例具有 then 方法，它的作用是为 promise 实例添加状态改变时的回调函数。then 方法返回的是一个新 promise 实例（与原来不同），可以采用链式写法。当第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

### Promise.prototype.catch()

用于指定发生错误时的回调函数。  
promise 对象的错误具有冒泡的性质，会一直向后传递，直到被捕获为止。错误总会被下一个 catch 捕获。

```
Promise
    .then(data => {
        //success
    })
    .catch(error => {
        //error
    });
```

promise 对象抛出的错误不会传递到外层代码。

### Promise.prototype.finally()

finally()方法用于指定不管 promise 对象最后状态如何，都会执行的操作。

```
promise
    .then(result => {...})
    .catch(error => {...})
    .finally(() => {...});
```

不论 promise 最后的状态如何，在执行完 then 或者 catch 指定的回调函数之后，都会执行 finally 方法指定的回调函数。

```
server.listen(port)
    .then(() => {...})
    .finally(server.stop);
```

finally 方法的回调函数不接受任何参数，这意味着没有办法知道前面 promise 状态是 fulfilled 还是 rejected。表明 finally 方法里面的操作与状态无关，不依赖 promise 的执行结果。  
finally 本质上是 then 方法的特例。

```
promise
    .finally(() => {...});
//等同于
promise
    .then(
        result => {
            ...
            return result;
        },
        error => {
            ...
            throw error
        }
    );
```

上例不使用 finally 方法，同样的语句需要在成功失败回调中各写一次。

### Promise.all()

该方法用于将多个 promise 实例包装为一个新的 promise 实例。  
`const p = Promise.all([p1,p2,p3]);`  
Promise.all 方法接受一个数组作为参数，p1，p2，p3 都是 Promise 的实例，或者先将参数使用 Promise.resolve 方法转换为 promise 实例。all 方法的参数可以不是数组，但必须具有 Iterator 接口（可迭代），且返回的每个成员都是 promise 实例。  
p 状态由参数决定，分两种情况：

1. 只有参数的状态都变为 fulfilled，p 的状态才会变为 fulfilled，此时参数的返回值组成一个数组，传递给 p 的回调函数。
2. 只要参数之中有一个被 rejected，p 的状态就变为 rejected，此时第一个被 reject 的实例返回值，会传递给 p 的回调函数。  
   **注意：**  
   如果作为参数的 promise 实例定义了自己的 catch 方法，一旦它被 rejected，并不会触发 all 的 catch 方法。

```
const p1 = new Promise((resolve, reject) => {
        resolve('hello');
    })
    .then(result => result)
    .catch(e => e);

const p2 = new Promise((resolve, reject) => {
    throw new Error('报错了');
})
    .then(result => result)
    .catch(e => e);

Promise.all([p1, p2])
    .then(result => console.log(result))
    .catch(e => console.log(e));
    // ["hello", Error: 报错了]
```

p1 会 resolve，p2 首先 rejected，但是 p2 有自己的 catch 方法，该方法返回的是一个新的 promise 实例，p2 指向的是这个实例。该实例执行完 catch 方法后也会变为 resolve，导致 all 方法参数里面两个实例都会 resolve，因此会调用 then 方法指定的回调函数，而不会调用 catch 方法指定的回调函数。  
若 p2 没有自己的 catch 方法，就会调用 all 的 catch 方法。

### Promise.race()

该方法是将多个 promise 实例包装为一个新的 promise 实例。  
`const p = Promise.race([p1,p2,p3]);`  
只要 p1，p2，p3 之中有一个实例率先改变状态，p 的状态紧接着改变，率先改变的 promise 实例的返回值，就传递给 p 的回调函数。  
promise.race()方法的参数和 promise.all 方法一样，如果不是 promise 实例，就会先调用下面讲到的 promise.resolve()方法，将参数转为 promise 实例，再进一步处理。

### Promise.allSettled()

该方法接受一组 promise 实例作为参数，包装为一个新的 promise 实例，只有等到所有的这些参数实例都返回结果，不管 fulfilled 还是 rejected，包装实例才会结束。

```
const promises = [
  fetch('/api-1'),
  fetch('/api-2'),
  fetch('/api-3'),
];

await Promise.allSettled(promises);
removeLoadingIndicator();
```

以上代码对服务器发出三个请求，等到三个请求都结束后，不论请求成功或者失败，加载的滚动图标就会消失。  
该方法返回新的 promise 实例，一旦结束状态总是 fuldilled，不会变为 rejected，状态变成 fulfilled 后，promise 的监听函数接收到的参数就是一个数组，每个成员对应接入一个 promise.allSettled()的 promise 实例。
上述方法的返回值 allSettledPromise，状态只可能变成 fulfilled，它的监听函数接收到的参数是数组 results。该数组的每个成员都是一个对象，对应传入 promise.allSettled()的两个 promise 实例，每个对象都有 status 属性，该属性的值只可能是字符串 fulfilled 或者字符串 rejected。fulfilled 时，对象有 value 属性，rejected 时有 reason 属性，对应两种状态的返回值。  
Promise.allSettled()方法可以确保所有的操作结束。

### Promise.any()

Promise.any()方法接受一组 Promise 实例作为参数，包装为一个新的 Promise 实例，只要实例变为 fulfilled 状态，包装的实例就会变成 fulfilled 状态；如果所有的参数实例变成 rejected 状态，包装实例就会变为 rejected 状态。
Promise.any()不会因为某个 promise 变成 rejected 状态而结束。

```
const promises = [
  fetch('/endpoint-a').then(() => 'a'),
  fetch('/endpoint-b').then(() => 'b'),
  fetch('/endpoint-c').then(() => 'c'),
];
try {
  const first = await Promise.any(promises);
  console.log(first);
} catch (error) {
  console.log(error);
}
```

Promise.any()方法的参数包含三个 promise 操作,其中只要有一个变成 fulfilled，Promise.any()返回的 Promise 对象就会变成 fulfilled，如果所有三个操作都变成 rejected，那么 await 命令就会抛出错误。 Promise.any()抛出的错误是一个 AggregateError 实例，相当于一个数组，每个成员对应一个被 rejected 操作所抛出的错误。

### Promise.resolve()

该方法可以将现有对象转为 promise 对象。  
`const jsPromise = Promise.resolve(...);`  
`Promise.resolve('foo')`  
等价于  
`new Promise(resolve => resolve('foo'))`  
Promise.resolve()参数分为四种情况。

1. 参数是一个 Promise 实例  
   不做任何更改，直接返回这个实例。
2. 参数是一个 thenable 对象  
   thenable 对象是指的具有 then 方法的对象。

```
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
```

Promise.resolve()方法会将这个对象转为 Promise 对象，然后就立即执行 thenable 对象的 then()方法。

```
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function (value) {
  console.log(value);  // 42
});
```

thenable 对象的 then 方法执行后，对象 p1 的状态就变为 resolved，从而立即执行最后那个 then 方法指定的回调函数，输出 42。  
3. 参数不是具有 then 方法的对象，或根本不是对象  
如果参数是一个原始值，或者是一个不具有 then 方法的对象，则 promise.resolve()方法返回一个新的 promise 对象，状态为 resolved。

```
const p = Promise.resolve('Hello');

p.then(function (s) {
  console.log(s)
});
// Hello
```

新生成的 promise 对象 p，由于字符串 hello 不属于异步操作(判断方法是字符串对象不具有 then 方法)，返回 promise 实例的状态从一生成就是 resolved，所以回调函数会立即执行，promise.resolve 方法的参数会同时传给回调函数。  
4. 不带参数  
这回直接返回一个 resolved 的状态的 promise 对象。

### Promise.reject()

该方法会返回一个新的 promise 实例，该实例的状态为 rejected。  
`const p = Promise.reject('error');`  
等同于

```
const p = new Promise((resolve,reject) => reject('error'))
p.then(null, s => console.log(s));
//error
```

代码生成一个 promise 对象的实例 p，状态为 rejected，回调函数立即执行。  
Promise.reject()方法的参数，会原封不动地作为 reject 的理由，变为后续方法的参数。

### Promise.try()

若不想知道或者不区分函数是同步还是异步，但是想用 promise 去处理，这样就可以不管函数是否包含异步操作，都使用 then 方法指定下一步流程，用 catch 方法处理函数抛出的错误。

```
const f = () => console.log('now');
Promise.try(f);
console.log('next');
//now
//next
```

使用 promise.try 包装一下，可以更好的管理异常。

## async 函数

### 含义

async 函数就是 Generator 的语法糖

```
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

1. 内置执行器  
   Generator 函数执行必须依靠执行器，所有有了 co 模块，而 async 函数自带执行器，async 函数的执行，与普通函数一样，只要一行。
2. 更好语义
3. 更广的适用性  
   async 函数的 await 命令后面，可以是 promise 对象和原始类型的值（数值，字符串，布尔值，此时会自动转成立即 resolved 的 promise 对象）。
4. 返回值是 promise  
   async 函数的返回值是 promise 对象，可以使用 then 方法指定下一步的操作。

### 用法

async 函数返回一个 promise 对象，可以使用 then 方法添加回调函数，当函数执行的时候，一旦遇到 await 就会先返回，等到异步操作完成，继续执行函数体内的语句。

### 语法

async 函数内部 return 语句返回的值，会成为 then 方法回调函数的参数。

```
async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"
```

async 函数内部抛出错误，会导致返回的 promise 对象变为 rejected 状态，抛出错误对象会被 catch 方法回调函数接收到。

```
async function f() {
  throw new Error('出错了');
}

f().then(
  v => console.log('resolve', v),
  e => console.log('reject', e)
)
//reject Error: 出错了
```

async 函数返回的 promise 对象，必须等到内部所有 await 命令后面的 promise 对象执行完，才会发生状态改变，除非遇到 return 语句或者返回错误。

await 命令后面是一个 promise 对象，返回该对象的结果，若不是 promise 对象，就直接返回对应值。  
另一种情况是，await 命令后面是一个 thenable 对象（即定义了 then 方法的对象），那么 await 会将其等同于 promise 对象。

### async 函数的实现原理

async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。

```
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function* () {
    // ...
  });
}
```

### 比较

简洁符合语义。

### 按顺序完成异步操作

一组异步操作，需要按照顺序完成，远程读取一组 url，然后按照读取顺序输出结果。

```
async function logInOrder(urls) {
  // 并发读取远程URL
  const textPromises = urls.map(async url => {
    const response = await fetch(url);
    return response.text();
  });

  // 按次序输出
  for (const textPromise of textPromises) {
    console.log(await textPromise);
  }
}
```  
虽然map方法的参数是async函数，但它是并发执行的，因为只有async函数内部是继发执行，外部不受影响。后面的for..of循环内部使用了await，因此实现了按顺序输出。  

### 顶层 await
await命令只能出现在async函数内部。  
提案允许在模块的顶层独立使用await命令。目的是借用await解决模块异步加载的问题。  
顶层await命令交出代码的执行权给其他模块加载，等异步操作完成后，再拿回控制权。  

