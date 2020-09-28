## 数组的扩展

### 扩展运算符

该运算符将一个数组变为参数序列。  
可以替代函数的 apply 方法。

```
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);
```

通过 push 函数，将一个数组添加到另一个数组尾部。

```
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```

扩展运算符的应用

1. 复制数组  
   数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。  
   `const a1 = [1, 2];`  
   `const a2 = [...a1];`
2. 合并数组  
   扩展运算符提供了数组合并的方法。

```
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

方法为浅拷贝，需要注意。

```
const a1 = [{ foo: 1 }];
const a2 = [{ bar: 2 }];

const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];

a3[0] === a1[0] // true
a4[0] === a1[0] // true
```

a3,a4 是用两种不同方法合并而成的新数组，但它们的成员都是对原数组成员的引用，这就是浅拷贝，如果修改了引用指向的值，会同步反映到新数组。

3. 与解构赋值的结合
   扩展运算符可以与解构赋值结合，用于生成新数组。
4. 字符串  
   扩展运算符可以将字符串转为真正的数组。

```
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

扩展运算符可以正确的识别四个字节的 Unicode 字符。  
5. 实现了 iterator 接口的对象  
任何定义了遍历器（Iterator）接口的对象，都可以用扩展运算符转为真正的数组。  
6. Map 和 Set 结构，Generator 函数  
扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。

```
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);
let arr = [...map.keys()]; // [1, 2, 3]
```

### Array.from

Array.from 方法用于将类似数组的对象和可遍历的对象转为真正的数组。

```
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

Array.from 还可以接受第二个参数，用来对每个元素进行处理，将处理后的值放入返回的数组。

```
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```

### Array.of

用于将一组值转化为数组。

```
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```

Array.of 可以用来替代 Array 和 new Array，并且不存在由于参数不同而导致的重载。

### 数组实例的 copyWhthin

在当前数组的内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。  
三个参数：

- target（必须）：从该位置开始替换数据，负值表示倒数。
- start（可选）： 从该位置开始读取数据，负值从尾部开始。
- end（可选）： 到该位置前停止读取数据，默认等于数组的长度，负值表示从结尾计算。  
  `[1, 2, 3, 4, 5].copyWithin(0, 3)`  
  `// [4, 5, 3, 4, 5]`

### 数组实例的 find 和 findIndex

find 方法用于找出第一个符合条件的数组成员，参数是一个回调函数，所有的成员依次执行该回调函数，直到找出第一个返回值为 true 的成员，然后返回该成员，若没有符合条件的成员，则返回 undefined。  
`[1, 4, -5, 10].find((n) => n < 0) //-5`  
find 方法的回调函数可以接受三个参数，以此为当前的值，当前的位置和原数组。

```
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

findIndex 用法与 find 相似，返回第一个符合条件的数组成员的位置。若都不符合则返回-1.

```
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

这两个方法都接受第二个参数，用来绑定回调函数的 this 对象。

### 数组实例的 fill

fill 方法使用给定值，填充一个数组。

```
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```

fill 用于空数组的初始化非常方便，数组中已有的元素会被全部抹去。  
fill 方法还可以接受第二个第三个参数，用于指定填充的起始位置和结束位置。  
**注意：**  
如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。

### 数组实例的 entries，keys，values

三个方法用于遍历数组，都返回一个遍历器对象。可以用 for..of 循环进行遍历，唯一的区别是 keys 是对键名的遍历，values 是对键值的遍历，entries 是对键值的遍历。

```
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

### 数组实例的 includes

返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。

```
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```

方法的第二个参数表示搜索的起始位置，默认为 0.若第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度，则会重置为 0 开始。  
此前使用 indexof 方法检查是否包含某个值。  
indexof 有两个缺点，一是不够语义化，二是使用严格运算符判断，会导致对 NaN 的误判。  
`[NaN].indexOf(NaN) // -1`  
includes 使用的是不一样的判断算法，就没有这个问题。  
`[NaN].includes(NaN) // true`  
**注意：**  
Map 与 Set 数据结构有一个 has 方法，需要注意与 includes 区分。

- Map 结构的 has 方法，是用来查找键名的
- Set 结构的 has 方法是用来查找值的。

### flat，faltMap

数组成员有时还是数组，flat 用于将嵌套的数组拉平，变为一维数组，该方法返回新数组。  
`[1,2,[3,4]].flat() //[1,2,3,4]`  
flat 默认拉平一层，若想要拉平多维嵌套数组，可以将 flat 方法的参数写成一个整数，默认为 1.

```
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
```

不论嵌套多少层，都要转为一维，可以用 Infinity 关键字作为参数。

```
[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]
```

若数组有空位，flat 则会跳过空位。  
flatMap 方法对原数组的每个成员执行一个函数，然后对返回值组成的数组执行 flat 方法。该方法返回一个新数组，不改变原数组。

```
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```

flatMap 只能展开一层数组。  
flatMap()方法的参数是一个遍历函数，该函数可以接受三个参数，分别是当前数组成员、当前数组成员的位置（从零开始）、原数组。

```
arr.flatMap(function callback(currentValue[, index[, array]]) {
  // ...
}[, thisArg])
```

### 数组的空位

数组空位指数组的某一个位置没有值。  
**注意：**  
空位不是undefined，一个位置的值等于undefined，依然是有值的。空位是没有任何值。  
- forEach(), filter(),reduce(),every(),some()都会跳过空位。  
- map()会跳过空位，但会保留这个值。  
- join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。  
```
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```  
第一个数组的0号位置是有值的，第二个数组的0号位置没有值。  
```
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// reduce方法
[1,,2].reduce((x,y) => x+y) // 3

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```  
ES6明确将空位转为undefined。  
Array.from()方法会将数组的空位转为undefined。  
扩展运算符也会将空位转为undefined。  
copyWithin()会连空位一起拷贝。  
fill()会将空位是为正常的数组位置。  
for..of循环也会遍历空位。  
```
let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1
```  
数组arr有两个空位，并没有被忽略。  
entries，keys，values，find，findIndex会将空位处理为undefined。  
### Array.prototype.sort 的排序稳定性

## 对象的扩展

### 属性简洁表示法

ES6 允许在大括号中直接写入变量和函数。  
除了属性，方法也可以简写。

### 属性名表达式

ES6 允许字面量定义对象时，可以将表达式放在方括号内。  
表达式还可以用于定义方法名。

### 方法的 name 属性

函数的 name 属性，返回属性名，对象方法也是函数，因此也有 name 属性。  
若对象的方法使用了取值函数 setter 和 getter，则 name 属性是在该方法的属性的描述对象的 set 和 get 上面，返回值是方法名前加上 get 和 set。

### 属性的可枚举性和遍历

对象的每个属性都有一个描述对象，用来控制该属性的行为。  
`Object.getOwnPropertyDescriptor`方法可以获取该属性的描述对象。  
描述对象的 enumerable 属性，称为可枚举性，若该属性为 false，就表示某些操作会忽略当前属性。  
有四个操作会忽略 enumerable 为 false 属性。

- for in 循环：只遍历对象自身和继承的可枚举属性。
- Object.keys： 返回对象自身的所有可枚举的属性的键名。
- JSON.stringify: 只串行化自身对象的可枚举属性。
- Object.assign: 忽略 enumerable 为 false 的属性，只拷贝对象自身的可枚举属性。  
  最后一个方法是 ES6 新增，只有 for in 会返回继承的属性，其他方法只处理对象自身的属性。  
  最初引入可枚举性的目地是让某些属性可以规避掉 for in 操作，不然所有内部的属性和方法都会被遍历到。  
  ES6 共有五种遍历方法：

1. for in  
   循环遍历对象自身的和继承的可枚举属性。
2. Object.keys(obj)
   该方法返回一个数组，包括对象自身的（不含继承的）所有可枚举属性的键名。
3. Object.getOwnpropertyNames(obj)  
   该方法返回一个数组，包含对象自身的所有属性（不含 symbel，但包括不可枚举属性）的键名。
4. Object.getOwnPropertySymbols(obj)
   该方法返回一个数组，包含对象自身所有的 Symbol 属性的键名。
5. Reflect.ownKeys(obj)
   该方法返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或者字符串，也不管是否可以枚举。  
   遍历规则：  
   首先遍历所有数值键，按照数值上升序排列。  
   其次遍历所有字符串键，按照加入时间升序排列。  
   最后遍历所有的 Symbol 键，按照加入时间升序排列。

### super 关键字

this 关键字总是指向函数所在的当前对象，ES6 新增一个类似的关键字 super，指向当前对象的原型对象。  
super 关键字表示原型对象时，只能用在对象的方法之中。  
js 引擎内部，super.foo 等同于 Object.getProtptypeOf(this).foo 属性或 Object.getPrototypeOf(this).foo.call(this)（方法）。

### 对象的扩展运算符

解构赋值：  
对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的尚未被读取的属性，分配到指定的对象上面。所有的键和值，都会拷贝到新对象上面。  
解构赋值要求等号右边是一个对象，如果等号右边是 undefined 或 null，就会报错，因为无法转为对象。  
结构赋值必须是最后一个参数。  
**注意：**  
解构赋值的拷贝是浅拷贝，如果一个键的值是复合类型（数组，对象，函数），那么解构赋值拷贝的就是这个值的引用，而不是这个值的副本。  
扩展运算符的解构赋值不能复制继承原型对象的属性。

### 链判断运算符

如果读取对象内部的某个属性，往往需要判断一下该对象是否存在。  
三元运算符？：常用于判断对象是否存在。

```
const fooInput = myForm.querySelector('input[name=foo]')
const fooValue = fooInput ? fooInput.value : undefined
```

这样层层判断很麻烦，因此 ES2020 引入链判断运算符。

```
const firstName = message?.body?.user?.firstName || 'default';
const fooValue = myForm.querySelector('input[name=foo]')?.value
```

?.运算符直接在链式调用的时候判断，左侧的对象是否为 null 或 undefined，如果是这样则不向下运算。二十返回 undefined。  
`iterator.return?.()`  
如果 iterator.return 有定义，就会调用该方法，否则直接返回 undefined，不再执行?.后面的部分。  
对于那些可能没实现的方法，这个运算符很有用。  
链式判断运算符有三种用法。

1. obj?.prop //判断对象属性
2. obj?.[expr] //同上
3. func?.(...args) //函数或者对象方法的调用

```
a?.b
// 等同于
a == null ? undefined : a.b

a?.[x]
// 等同于
a == null ? undefined : a[x]

a?.b()
// 等同于
a == null ? undefined : a.b()

a?.()
// 等同于
a == null ? undefined : a()
```

**注意：**

1. 短路机制  
   ?.运算符相当于一种短路机制，只要条件不满足，就不再执行。
2. delete 运算符

```
delete a?.b
// 等同于
a == null ? undefined : delete a.b
```

如果 a 为 undefined 或者 null，会直接返回 undefined，不会进行 delete 运算。  
3. 括号影响  
若属性链有圆括号，链判断运算符对圆括号外部没有影响，只对圆括号内部有影响。

```
(a?.b).c
// 等价于
(a == null ? undefined : a.b).c
```

使用链式运算符时，不应该使用圆括号。  
4. 报错

```
// 构造函数
new a?.()
new a?.b()

// 链判断运算符的右侧有模板字符串
a?.`{b}`
a?.b`{c}`

// 链判断运算符的左侧是 super
super?.()
super?.foo

// 链运算符用于赋值运算符左侧
a?.b = c
```

5. 右侧不得为十进制  
   若判断符后面紧跟着十进制数字，则会按照三元运算符处理，小数点会归属于后面的十进制数字。

### NULL 判断运算符

读取对象属性的时候，如果某个属性的值是 null 或 undefined，有时需要为它们指定默认值。常见是通过||运算符指定默认值。  
但左侧为属性为空字符串或者 false 0 的时候，默认值也会生效。  
ES2020 引入全新 null 判断运算符？？。行为类似||，只有运算符左侧值为 null 或 undefined 时，才会返回右侧的值。

```
const headerText = response.settings.headerText ?? 'Hello, world!';
const animationDuration = response.settings.animationDuration ?? 300;
const showSplashScreen = response.settings.showSplashScreen ?? true;
```

??与&&和||优先级不好判断，多个逻辑运算符一起使用时应该用括号表示优先级。

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

虽然 map 方法的参数是 async 函数，但它是并发执行的，因为只有 async 函数内部是继发执行，外部不受影响。后面的 for..of 循环内部使用了 await，因此实现了按顺序输出。

### 顶层 await

await 命令只能出现在 async 函数内部。  
提案允许在模块的顶层独立使用 await 命令。目的是借用 await 解决模块异步加载的问题。  
顶层 await 命令交出代码的执行权给其他模块加载，等异步操作完成后，再拿回控制权。
