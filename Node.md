## fs  
### 文件描述符  
文件描述符是使用fs模块提供的open方法打开文件后返回的。  
`const fs = require('fs');`  
`fs.open('路径', 'r')`  
**注意：**  
将r作为第二个参数意味着打开文件只用于读取。  
- r+打开文件用于读写  
- W+打开文件用于读写，将流定位到文件开头。若不存在将创建文件。  
- a打开文件用于写入，将流定位到文件的末尾。若不存在将创建文件。  
- a+打开文件用于读写，将流定位到文件的末尾。若不存在将创建文件。  

使用fs.openSync()方法打开文件，该方法会返回文件描述符。  
一旦获取到文件描述符，就可以以任何方式执行所需要它的操作，例如调用fs.open以及许多文件系统交互的其他操作。  
### 文件属性  
每个文件都带有一组详细信息，可以使用node进行检查。  
使用fs提供的stat方法。  
调用时传入文件路径，一旦获取到文件信息，则会调用传入的回调函数，并带上两个参数：错误消息和文件属性。  
文件的信息包含在属性变量中，可以获取到以下信息：  
- isFile()和isDirectory()判断是否为文件或者目录。  
- isSymbolicLink()判断文件是否符号连接。  
- size获取文件大小。  

## path  
### 文件路径  
`const path = require('path')`  
给定路径可以使用方法提取信息。  
- dirname：获取文件的父文件夹  
- basename：获取文件名部分  
- extname： 获取文件扩展名  

`path.join()`连接路径的两个或多个片段  
`path.resolve()`获得相对路径的绝对路径计算。  
`path.normalize()`可以计算包含. .. 斜杠之类的实际路径  
### 读取文件  
`fa.readFile()`方法读取文件，可以传入文件路径，编码，文件数据以及错误进行调用的回调函数。  
这个函数会在返回数据之前将文件的全部内容读取到内存中，这意味着会对内存的消耗和程序的执行速度有很大的影响。  
此时最好是使用流来读取文件。  
### 写入文件  
`fs.writeFile()`写入文件。  
默认情况写会替换文件的内容。（若文件已经存在）  
可以通过指定标志来修改默认的行为。  
`fs.writeFile('/Users/joe/test.txt', content, { flag: 'a+' }, err => {})`  
- `r+` 打开文件用于读写。  
- `w+` 打开文件用于读写，将流定位到文件开头，若文件不存在则创建文件。  
- `a` 打开文件用于写入，将流定位到文件的末尾。如果文件不存在则创建文件。  
- `a+` 打开文件用于读写，将流定位到文件的末尾。如果文件不存在则创建文件。  
### 追加到文件末尾  
`fs.appendFile()`可以将内容追加到末尾。  
### 使用流  
以上方法都是将全部内容写入文件之后才会将控制权返回给程序。更好的方法是使用流写入文件内容。    
## evnet  
events模块提供了EventEmitter类，可以处理事件。  
```
const EventEmitter = require('events')
const door = new EventEmitter()
```  
事件监听器返回及使用以下事件：  
- 当监听器被添加时返回newListener。  
- 当监听器被移除时返回removeListener。  
常用方法说明：  
1. `emitter.addListener()`  
emitter.on()别名。  
2. `emitter.emit()`  
触发事件，按照事件被注册的顺序同步地调用每个事件监听器。  
3. `emitter.evnetNames()`  
返回字符串（表示在当前EventEmitter对象上注册的事件）数组。  
4. `emitter.getMaxListener()`  
获取可以添加到EventListener对象的监听器的最大数量（默认为10，可以使用setMaxListener进行增加或者减少）  
5. `emitter.listenerCount()`  
获取作为参数传入的事件监听器的计数。  
6. `emitter.listeners()`  
获取作为参数传入的事件监听器数组。  
7. `emitter.off()`  
emitter.removeListener()的别名。  
8. `emitter.on`  
添加当前事件被触发时调用的回调函数。  
9. `emitter.once()`  
添加当事件在注册之后首次触发时调用的回调函数。该回调函数只会被回调一次，不会再被回调。  
10. `emitter.prependListener()`  
当使用on或addListener添加监听器时，监听器会被添加到监听队列中的最后一个，并且最后一个调用。使用prependListener则可以在其他监听器之前添加并调用。  
11. `emitter.prependOnceListener()`  
当使用once添加监听器时，监听器会被添加到监听队列中的最后一个，并且最后一个被调用，使用prependOnceListener则可以在其他监听器之前添加并调用。  
12. `emitter.removeAllListener()`  
移除所有EventEmitter对象的监听特定事件的监听器。  
13. `emitter.removeListener()`  
移除特定的监听器，可以通过将回调函数保存到变量中，一边后续引用。  
14. `emitter.setMaxListener()`  
设置可以添加到EventEmitter对象的监听器的最大数量（默认为10，可以增加或减少）。  


