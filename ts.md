## typeScript  
### 1.基础类型  
- 布尔型：`let isDone: boolean = false;`  
- 数字：浮点数类型，关键词为number，ts支持二进制与八进制。  
`let decLiteral: number = 6;`  
`let hexLiteral: number = 0xf00d;`  
`let binaryLiteral: number = 0b1010;`  
`let octalLiteral: number = 0o744;`  
- 字符串：可以使用(',")  
`let name: string = "bob";`  
`name = "smith";`  
也可以使用模板字符串。  
- 数组：  
定义有两种方式：
1. 可以在元素类型后面接上 []，表示由此类型元素组成的一个数组。  
`let list: number[] = [1, 2, 3];`  
2. 第二种方式是使用数组泛型，Array<元素类型>  
`let list: Array<number> = [1, 2, 3];`  
- 元组Tuple：  
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。  
`// Declare a tuple type`  
`let x: [string, number];`  
`// Initialize it`  
`x = ['hello', 10]; // OK`  
`// Initialize it incorrectly`  
`x = [10, 'hello']; // Error`  
当访问一个越界的元素，将会使用联合类型替代。  
- 枚举：  
enum类型是对JavaScript标准数据类型的一个补充。  
`enum Color {Red, Green, Blue}`  
`let c: Color = Color.Green;`  
默认情况下，从0开始编号，也可以手动指定成员的值。  
- Any:   
在编程阶段还不清楚类型的变量指定一个类型。这些值可能来自于动态的类型。这种情况下，我们希望这些变量可以直接通过检查。可以使用any来标记这些变量。  
`let notSure: any = 4;`  
`notSure = "maybe a string instead";`  
`notSure = false; // okay, definitely a boolean`  
当只知道部分数据时，any类型很有用。  
`let list: any[] = [1, true, "free"];`  
`list[1] = 100;`  
- void:  
它没有表示任何类型，当一个函数没有返回值时，函数定义为void。  
`function warnUser(): void {
    console.log("This is my warning message");
}`  
默认情况下，null和undefined是所有类型的子类型。就是说可以将null和undefined赋值给number类型的变量。  
然而当你指定了--strictNullChecks标记，null和undefined只能赋值给void和他们自己。这可以避免很多常见的问题。  
- never：  
表示那些永远不存在的值。例如never类型是那些总会抛出异常或者根本不会有返回值的函数表达式或箭头函数表达式的返回类型；变量也有可能是never类型，当它们被永不为真的类型保护所约束时。  
never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never类型本身）。即使any也不可以赋给never。  
`// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}`  
- Object:  
object表示非原始类型，也就是除number，string，Boolean，symbol，null和undefined之外的类型。  
使用object类型，就可以更好的表示像object.create这样的API。  
`declare function create(o: object | null): void;
create({ prop: 0 }); // OK
create(null); // OK
create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error`  
- 类型断言：  
通过类型断言这种方式告诉编译器某个值的具体类型。类型断言和其他语言中类型转换相似，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。  
类型断言有两种形式：
1. 尖括号法：  
`let someValue: any = "this is a string";`  
`let strLength: number = (<string>someValue).length;`  
2. as语法：  
`let someValue: any = "this is a string";`  
`let strLength: number = (someValue as string).length;`  
- let: 
typeScript中尽量使用let代替var。  
### 2.变量声明  
#### 变量声明  
- var  
- let：在声明前使用会报错，ts会检查这种问题。  
#### var声明  
#### let声明  
##### 重定义及屏蔽   
var多次声明，只会得到离引用最近的一次定义，let不允许多次声明。
不同块中可以定义相同变量名称。  
`function f(x) {
    let x = 100; // error: interferes with parameter declaration
}`  
`function g() {
    let x = 100;
    var x = 100; // error: can't have both declarations of 'x'
}`  
在嵌套作用内引入一个新名字的行为称作屏蔽。  
`function f(condition, x) {
    if (condition) {
        let x = 100;
        return x;
    }
    return x;
}`  
`f(false, 0); // returns 0`  
`f(true, 0);  // returns 100`  
##### 块级作用域变量的获取  
在获取用var声明的变量时，每次进入一个作用域时，它创建了一个变量的环境。就算作用域内代码已经执行完毕，这个环境与其捕获的变量依然存在。  
#### const声明  
const声明是声明变量的另一种形式。与let声明类似，它们被赋值后不能再改变。它们拥有与let相同的作用域规则，但是不能对它们重新赋值。实际上const变量的内部状态是可以更改的。ts允许你将对象成员设置为只读的。  
#### let vs const  
使用最小特权原则，所有的变量除了计划需要修改的都应该使用const。基本原则就是如果一个变量不需要对它写入，那么其他使用这个代码的人也不能够写入它们。  
#### 解构  
##### 解构数组  
数组的解构赋值：  
`let input = [1, 2];`  
`let [first, second] = input;`  
`console.log(first); // outputs 1`  
`console.log(second); // outputs 2`  
创建了两个变量，first和second。  
作用于函数参数:  
`function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}`  
`f(input);`  
可以在数组里使用...语法创建剩余变量：  
`let [first, ...rest] = [1, 2, 3, 4];`  
`console.log(first); // outputs 1`  
`console.log(rest); // outputs [ 2, 3, 4 ]`  
##### 对象解构  
结构对象  
`let o = {
    a: "foo",
    b: 12,
    c: "bar"
};`  
`let { a, b } = o;`  
此时通过o.a and o.b 创建了a和b，不需要c可以忽略。  
就像数组解构，可以使用未声明的赋值。  
可以在对象中使用...语法创建剩余变量。  
##### 属性重命名  
可以给属性以不同的名字。  
`let { a: newName1, b: newName2 } = o;`  
读作a作为newName1，方向从左至右。  
`let newName1 = o.a;`  
`let newName2 = o.b;`  
如果要指定类型，依然要写完整。  
`let {a, b}: {a: string, b: number} = o;`  
##### 默认值  
默认值可以在属性为undefined时使用缺省值。  
`function keepWholeObject
(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}`  
##### 函数声明  
解构也可以用于函数声明。  
##### 展开  
可以将一个数组展开为另一个数组，或将一个对象展开为另一个对象。  
`let first = [1, 2];`  
`let second = [3, 4];`  
`let bothPlus = [0, ...first, ...second, 5];`  
`bothPlus //[0,1,2,3,4,5]`  
展开操作创建了first和second的一份浅拷贝。它们不会被展开操作符所改变。  
展开对象：  
`let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };`  
`let search = { ...defaults, food: "rich" };`  
出现在展开对象后面的属性会覆盖前面的属性。  
对象展开还有一些限制。首先它仅包含对象的自身可枚举属性。当展开对象实例，会丢失其方法。  
`class C {
  p = 12;
  m() {
  }
}`  
`let c = new C();`  
`let clone = { ...c };`  
`clone.p; // ok`  
`clone.m(); // error!`  
### 3.接口  
typeScript的核心原则之一是对值所具有的结构进行类型检查。被称做“鸭式辨型法”或“结构性子类型化”。  
#### 接口  
`function printLabel(labelledObj: { label: string }) {
  console.log(labelledObj.label);
}`

`let myObj = { size: 10, label: "Size 10 Object" };`  
`printLabel(myObj); //Size 10 Object`  
在本例中，类型检查器会检查printLabel的调用，会检查参数label的类型是否为string，需要注意的是，传入的参数可能会有很多属性，编译器只会检查哪些必须的属性是否存在，并且其类型是否匹配。  
`interface LabelledValue {
  label: string;
}`  
`function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}`  
`let myObj = {size: 10, label: "Size 10 Object"};`  
`printLabel(myObj);`  
LabelledValue是一个名字，用来描述上面例子里的需求。它代表了一个label属性且类型为string的对象。只要传入的对象满足上面的条件，那就是被允许的。  
类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型正确即可。  
#### 可选属性  
接口中的属性不全都是必须的，可选属性在应用"option bags"模式时常用，即给函数传入的属性对象中只有部分属性赋值了。  
`interface SquareConfig {
  color?: string;
  width?: number;
}`  
`function createSquare(config: SquareConfig): 
{color: string; area: number} {
  let newSquare = {color: "white", area: 100};
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}`  
`let mySquare = createSquare({color: "black"});`  
带有可选属性的接口与普通接口类似，只是在可选属性名字定义后面加？。  
可选属性的好处是可以对可能存在的属性进行预定义。也可以捕获引用了不存在属性时的错误。  
#### 只读属性  
一些属性只能在对象刚刚创建的时候修改其值。可以在属性名前用readonly来指定只读属性。  
`interface Point {
    readonly x: number;
    readonly y: number;
}`  
可以通过赋值一个对象字面量来构造一个Point。赋值后，x与y再也不能被改变。  
`let p1: Point = { x: 10, y: 20 };`  
`p1.x = 5; // error!`  
TypeScript具有ReadonlyArray<T>类型，与Array<T>类似，只是将所有的可变方法去掉了，因此可以确保数组创建后再也不能被修改。  
`let a: number[] = [1, 2, 3, 4];`  
`let ro: ReadonlyArray<number> = a;`  
`ro[0] = 12; // error!`  
`ro.push(5); // error!`  
`ro.length = 100; // error!`  
`a = ro; // error!`  
虽然不可以将整个ReadonlyArray赋值到一个普通数组是不被允许的，但是可以使用类型断言重写：  
`a = ro as number[];`  
#### readonly 与 const  
最简单判断改用readonly还是const的方法就是要把它看作变量还是属性。作为变量使用的话用const，若作为属性则使用readonly。  
#### 额外的属性检查
`interface SquareConfig {
    color?: string;
    width?: number;
}`  
`function createSquare(config: SquareConfig): 
{ color: string; area: number } {
    // ...
}` 
`let mySquare = createSquare({ colour: "red", width: 100 });`  
在传递参数过程中，即使程序已经类型化，属性也兼容，但是当传递的参数有拼写错误的时候，typescript会认为这段代码有bug。这个对象字面量会被特殊对待而且会经过额外的属性检查，当它们赋值给变量或者作为参数传递的时候。如果对象字面量存在任何目标类型不包含的属性时，都会得到错误。  
`// error: 'colour' not expected in type 'SquareConfig'`  
`let mySquare = createSquare({ colour: "red", width: 100 });`  
绕开这些检查最简单的方法就是类型断言：  
`let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);`  
最佳的方式是能够添加一个字符串索引签名，前提是你能够确定这个对象可能具有作为某些特殊使用的额外属性。如果SquareConfig带有除上述定义的属性外还有任意数量的其他属性，则：  
`interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}`  
这里表示SquareConfig可以带有任意数量的属性，只要不是已经定义过的，无所谓它们的类型是什么。  
还有最后一种跳过这些检查的方法。就是将这个对象赋给另一个变量，因为squareOptions不会经过额外的检查，所以编译器不会报错。  
`let squareOptions = { colour: "red", width: 100 };`  
`let mySquare = createSquare(squareOptions);`  
#### 函数类型  
接口能够描述js中对象拥有的各种各样的外形。除了描述带有属性的普通对象外，接口也可以描述函数类型。  
为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表的每个参数都需要名字和类型。  
`interface SearchFunc {
  (source: string, subString: string): boolean;
}`  
这样我们就可以像使用其他接口一样使用这个函数类型的接口。  
创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。  
`let mySearch: SearchFunc;`  
`mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}`  
#### 可索引的类型  
与使用描述函数类型类似，可以描述那些能够通过索引得到的类型，可索引类型有一个索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。  
`interface StringArray {
  [index: number]: string;
}`  
`let myArray: StringArray;`  
`myArray = ["Bob", "Fred"];`  
`let myStr: string = myArray[0];`  
例子中定义了StringArray接口，它具有索引签名。这个索引签名表示当用number去索引StringArray时会得到string类型的返回值。  
ts支持两种索引签名，字符串和数字，可以同时使用两种签名索引。但是数字索引的返回值必须是字符串索引返回值类型的子类型，这是因为当使用number来索引时，js会将其转换为string然后再去索引对象。也就是说用100（一个number）去索引等同于使用"100"（一个string）去索引，因此两者需要保持一致。  
字符串索引类型可以很好的描述dictionary模式，并且它们也会确保所有属性与其返回值类型匹配。因为字符串索引声明了`obj.property`和`obj["property"]`两种形式都可以。下例中name类型与字符串索引类型不匹配。类型检查器会报错。   
可以将索引签名设置为只读属性，这样防止给索引赋值。  
`interface ReadonlyStringArray {
    readonly [index: number]: string;
}`  
`let myArray: ReadonlyStringArray = ["Alice", "Bob"];`  
`myArray[2] = "Mallory"; // error!`  
#### 类类型  
- 实现接口  
ts可以用它来明确的强制一个类去符合某种约定。  
可以在接口中描述一个方法，在类中实现。  
`interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}`  
`class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}`  
- 类静态部分与实例部分的区别  
当操作类和接口的时候，类有两种类型：静态部分的类型和实例的类型，当使用构造器签名去定义一个接口并试图定义一个类去实现这个接口时会得到一个错误。  
`interface ClockConstructor {
    new (hour: number, minute: number);
}`  
`class Clock implements ClockConstructor {
    currentTime: Date;
    constructor(h: number, m: number) { }
}`  
因为当一个类实现了一个接口时，只对其实例部分进行类型检查。constructor存在于类的静态部分，所以不在检查的范围内。  
因此应该直接操作类的静态部分。下面定义了两个接口，ClockConstructor为构造函数所用和ClockInterface为实例方法所用，为了方便我么定义一个构造函数createClock，它用传入的类型创建实例。  
`interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}`  
`interface ClockInterface {
    tick();
}`  
`function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}`  
`class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}`  
`class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}`  
`let digital = createClock(DigitalClock, 12, 17);`  
`let analog = createClock(AnalogClock, 7, 32);`  
因为createClock第一个参数是ClockConstructor类型，所以在createClock(DigitalClock, 12, 17)中会检查DigitalClock是否符合构造函数签名。  
- 继承接口  
和类一样，接口也可以相互继承。这能够从一个接口复制成员到另一个接口里，可以更加灵活的将接口分隔到可重用的模块里。  
`interface Shape {
    color: string;
}`  
`interface Square extends Shape {
    sideLength: number;
}` 
`let square = <Square>{};`  
`square.color = "blue";`  
`square.sideLength = 10;`  
一个接口可以继承多个接口，创建出多个接口的合成接口。  
`interface Shape {
    color: string;
}`  
`interface PenStroke {
    penWidth: number;
}`  
`interface Square extends Shape, PenStroke {
    sideLength: number;
}`  
`et square = <Square>{};`  
`square.color = "blue";`  
`square.sideLength = 10;`  
`square.penWidth = 5.0;`  
- 混合类型  
接口能够描述js里丰富的类型。因为js动态灵活的特点，有时会希望一个对象可以具有上面提到的多种类型。  
一个对象可以同时作为函数和对象使用，并且带有额外的属性。  
`interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}`  
`function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}`  
`let c = getCounter();`  
`c(10);`  
`c.reset();`  
`c.interval = 5.0;`  
- 接口继承类  
当接口继承了一个类类型时，它会继承类的成员但不包括其实现。就好像接口声明了类中存在的成员，但并没有提供具体的实现一样。接口同样会继承到类的private和protected成员，这意味着当创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。  
当具有一个庞大的继承结构时这很有用，但是要指出你的代码只在子类拥有特定属性时起作用。这个子类除了继承至基类外与基类没有任何关系。  
`class Control {
    private state: any;
}`  
`interface SelectableControl extends Control {
    select(): void;
}`  
`class Button extends Control implements SelectableControl {
    select() { }
}`  
`class TextBox extends Control { select() { } }`  
`// 错误：“Image”类型缺少“state”属性。`  
`class Image implements SelectableControl {
    select() { }
}`  
`class Location {}`  
例子中SelectableControl包含了Control的所有成员，包括私有成员state。因为state是私有成员，所以只能够是Control的子类才能实现SelectableControl接口。因为只有Control的子类才能拥有一个声明能够于Control的私有成员state，这对私有成员的兼容性是必须的。  
在Control类内部，是允许通过SelectableControl的实例来访问私有成员state的。实际上，SelectableControl接口和拥有select方法的Control类是一样的。Button和TextBox类是SelectableControl的子类（因为它们都继承自Control并有select方法），但Image和Location类并不是这样的。
### 4.类
传统js程序是使用函数和基于原型的继承来创建可复用的组件，从js6开始，js开发者可以使用基于类的面向对象的方式。  
#### 类  
`class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}`  
`let greeter = new Greeter("world");`  
声明一个Greeter类。这个类有三个成员，greeting属性，构造函数，greet方法。  
在引用任何一个类的成员的时候都用到了this，它表示我们访问的是类成员。  
最后一行使用new构造了Greeter的一个实例，它会调用之前定义的构造函数，创建一个Greeter类型的新对象，并执行构造函数初始化它。  
#### 继承  
在ts中可以经常使用面向对象模式，基于类的设计模式中一种最基本的模式是允许使用继承来扩展现有的类。  
`class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}`  
`class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}`  
`const dog = new Dog();`  
`dog.bark();`  
`dog.move(10);`  
`dog.bark();`  
类从基类中继承了属性和方法。在这里Dog是一个派生类，它来自于Animal基类，通过extends关键字。派生类通常被称为子类，基类通常被称为基类。因为Dog继承了Animal的功能，可以创建一个Dog的实例，调用bark和move方法。  
`class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}`  
`class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}`  
`class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}`  
`let sam = new Snake("Sammy the Python");`  
`let tom: Animal = new Horse("Tommy the Palomino");`  
`sam.move();`  
`tom.move(34);`  
本例使用extends关键字创建了两个Animal的子类，与前一个例子不同的是，派生类中包含了一个构造函数，它必须调用super（）方法，它会执行基类的构造函数。而且在构造函数里访问this的属性之前，一定要调用super（），这是ts中强制执行的一条规范。  
这个例子也演示了如何在子类里重写父类的方法。  
注意，即使 tom被声明为 Animal类型，但因为它的值是 Horse，调用 tom.move(34)时，它会调用 Horse里重写的方法。  

#### 公有私有受保护的修饰符  
- 默认为public  
在ts中成员默认为public。
- 理解private  
当成员被标记为private时，它就不能在声明它的类的外部访问。  
`class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}`  
`new Animal("Cat").name; // 错误: 'name' 是私有的.`  
ts使用的是结构性类型系统，当比较两种类型的时候，再不会在乎它们从何而来，如果所有的成员类型都是兼容的，我们就认为它们的类型是兼容的。  
当我们比较带有private或protected成员类型的时候，情况会有不同。如果其中一个类型包含一个private类型，那么只有当另一个类型中也存在private成员，并且它们都是来自于同一声明时，我们才会认为这两个类型是兼容的。protected也是如此。  
`class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}`  
`class Rhino extends Animal {
    constructor() { super("Rhino"); }
}`  
`class Employee {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}`  
`let animal = new Animal("Goat");`  
`let rhino = new Rhino();`  
`let employee = new Employee("Bob");`  
`animal = rhino;`  
`animal = employee; // 错误: Animal 与 Employee 不兼容.`  
不兼容的原因是在animal和employee中都有各自定义的private成员，不同的声明，所以不兼容。  
- 理解protected  
protected修饰符与private修饰符的行为类似，但是protected成员在派生类中仍然可以访问。  
`class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}`   
`class Employee extends Person {
    private department: string;
    constructor(name: string, department: string) {
        super(name)
        this.department = department;
    }
    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}`  
`let howard = new Employee("Howard", "Sales");`  
`console.log(howard.getElevatorPitch());`  
`console.log(howard.name); // 错误`  
**注：** 不能再Person类外使用name，但是可以通过employee类的实例方法访问，因为employee是由person派生而来。  
构造函数也可以被标记为protected，这意味着这个类不能再包含它的类以外被实例化，但是可以被继承。  
`class Person {
    protected name: string;
    protected constructor(theName: string) { this.name = theName; }
}`  
`// Employee 能够继承 Person`   
`class Employee extends Person {
    private department: string;
    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }
    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}`   
`let howard = new Employee("Howard", "Sales");`  
`let john = new Person("John"); // 错误: 'Person' 的构造函数是被保护的.`  
#### readonly修饰符  
可以使用readonly将属性设置为只读，只读属性必须在声明时或构造函数里被初始化。  
`class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}`  
`let dad = new Octopus("Man with the 8 strong legs");`  
`dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.`
- 参数属性  
再Octopus中定义了一个只读成员name和一个参数为theName的构造函数，并将theName赋值给name。参数属性可以方便的让我们在一个地方定义并者初始化一个成员。  
`class Octopus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {
    }
}`  
仅在构造函数中使用 `readonly name:string` 参数来创建和初始化name成员，将声明和赋值合并到一处。  
参数属性通过给构造函数前面添加一个访问限定符来声明。使用private限定一个参数属性会声明并初始化一个私有成员。  
#### 存取器  
ts支持通过setters/getters来截取对对象成员的访问。  
`class Employee {
    fullName: string;
}`  
`let employee = new Employee();`  
`employee.fullName = "Bob Smith";` 
`if (employee.fullName) {
    console.log(employee.fullName);
}`  
例子中可以随意设置fullName。  
`let passcode = "secret passcode";`  
`class Employee {
    private _fullName: string;
    get fullName(): string {
        return this._fullName;
    }
    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}`  
`let employee = new Employee();`  
`employee.fullName = "Bob Smith";`  
`if (employee.fullName) {
    alert(employee.fullName);
}`  
例子中先检查用户密码是否正确，再允许其修改员工的信息。将对fullName的直接访问改为可以检查密码的set方法。  
**注意：** 存取器要求将编译器设置为输出js5以上，其次只带有get不带有set的存取器被认作readonly。  
#### 静态属性
可以创建类的静态成员，这些属性存在于类本身而不是实例上，。本例中使用static定义origin，因为这是所有网格都会用到的属性。每个实例想要访问这个属性的时候都要再origin前面加上类名。如同在实例属性前使用this.前缀来访问一样。本例使用Grid.origin。  
`class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}`  
`let grid1 = new Grid(1.0);  // 1x scale`  
`let grid2 = new Grid(5.0);  // 5x scale`  
`console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));`  
`console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));`  
#### 抽象类  
抽象类做为其他派生类的基类使用，它们一般不会被实例化，不同于接口，抽象类可以包含成员的实现细节，abstract关键字是用于定义抽象类和在抽象类内部定义抽象方法。  
`abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}`  
抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。与接口方法类似，两者都是定义方法签名但不包含方法体，抽象方法必须包含abstract关键字并且可以包含访问修饰符。  
`abstract class Department {
    constructor(public name: string) {
    }
    printName(): void {
        console.log('Department name: ' + this.name);
    }
    abstract printMeeting(): void; // 必须在派生类中实现
}`  
`class AccountingDepartment extends Department {
    constructor() {
        super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
    }
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }
    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}`  
`let department: Department; // 允许创建一个对抽象类型的引用`  
`department = new Department(); // 错误: 不能创建一个抽象类的实例`  
`department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值`  
`department.printName();`  
`department.printMeeting();`  
`department.generateReports(); // 错误: 方法在声明的抽象类中不存在`  
#### 高级技巧  
- 构造函数  
当在ts中声明了一个类的时候，实际上声明了很多。首先就是类的实例的类型。  
`class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}`  
`let greeter: Greeter;`  
`greeter = new Greeter("world");`  
`console.log(greeter.greet());`  
在这里 `let greeter: Greeter` 意思是Greeter类的实例是Greeter。  
也创建了一个叫做构造函数的值，这个函数会在我们使用new操作符创建实例的时候被调用。  
`let Greeter = (function () {
    function Greeter(message) {
        this.greeting = message;
    }
    Greeter.prototype.greet = function () {
        return "Hello, " + this.greeting;
    };
    return Greeter;
})();`  
`let greeter;`  
`greeter = new Greeter("world");`  
`console.log(greeter.greet());`   
`let greeter;` 将被赋值为构造函数，当调用new并执行了这个函数后，便会得到一个类的实例。这个函数也包含了类的所有静态属性，换个角度说，我们可认为类具有实例部分与静态部分这两个部分。  
`class Greeter {
    static standardGreeting = "Hello, there";
    greeting: string;
    greet() {
        if (this.greeting) {
            return "Hello, " + this.greeting;
        }
        else {
            return Greeter.standardGreeting;
        }
    }
}`  
`let greeter1: Greeter;`  
`greeter1 = new Greeter();`  
`console.log(greeter1.greet());`  
`let greeterMaker: typeof Greeter = Greeter;`  
`greeterMaker.standardGreeting = "Hey there!";`  
`let greeter2: Greeter = new greeterMaker();`  
`console.log(greeter2.greet());`  
我们创建了一个greeterMaker的变量。这个变量保存了这个类或者说保存了类构造函数，然后使用typeof Greeter，意思是取Greeter类的类型，而不是实例的类型。或者更确切的说，告诉我Greeter标识符的类型，也就是构造函数的类型。这个类型包含了类的所有静态成员和构造函数。  
- 把类当作接口使用  
类定义会创建两个东西，类的实例类型和一个构造函数。因为类可以创建出类型，所以你可以在允许使用接口的地方使用类。  
`class Point {
    x: number;
    y: number;
}`  
`interface Point3d extends Point {
    z: number;
}`  
`let point3d: Point3d = {x: 1, y: 2, z: 3};`  
### 5.函数  
#### 介绍  
函数是js应用程序的基础，可以帮助你实现抽象层，模拟类，信息隐藏和模块，在ts中虽然已经支持类，命名空间和模块，但函数仍是主要定义行为的地方。  
#### 函数  
和js一样，ts也可以创建有名字的函数和匿名函数。  
在js中，函数可以使用函数体外部的变量，当函数这样做时，就相当于它捕获了这个变量。  
#### 函数类型  
- 为函数定义类型  
`function add(x: number, y: number): number {
    return x + y;
}`  
`let myAdd = function(x: number, y: number): number { return x + y; };`  
可以给每个参数添加类型之后再为函数本身添加返回值类型。  
- 书写完整函数类型  
`let myAdd: (x: number, y: number) => number =
function(x: number, y: number): number { return x + y; };`  
函数类型包含两个部分：参数类型和返回值类型。当写出完整函数类型的时候，这两部分都需要。以参数列表的形式写出参数类型，为每个参数指定一个类型和名字。也可以这样书写：  
`let myAdd: (baseValue: number, increment: number) => number =
function(x: number, y: number): number { return x + y; };`  
只要参数类型匹配，就认为它是有效函数类型，而不在乎函数名是否正确。  
第二部分是返回值的类型，对于返回值，在函数和返回值之间使用（=>）符号。之前提到，返回值类型是函数类型的必要部分，如果函数没有返回值，也必须指定返回值的类型是void而不能留空。  
函数的类型是由参数类型和返回值组成的，函数中使用的捕获变量不会体现在类型里。
- 推断类型  
如果在赋值语句一边指定了类型，另一边没有指定，ts编译器会自动识别出类型。  
`// myAdd has the full function type`  
`let myAdd = function(x: number, y: number): number { return x + y; };`  
`// The parameters `x`  and  `y` have the type number`  
`let myAdd: (baseValue: number, increment: number) => number = function(x, y) { return x + y; };`  
这叫做按上下文归类，是类型推论的一种。  
#### 可选参数和默认参数  
ts里的每个参数都是必须的。这不是指不能传递null或者undefined作为参数，而是说编译器检查用户是否为每个参数都传入了值，编译器还会假设只有这些参数会被传递进参数。  
`function buildName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
}`  
`let result1 = buildName("Bob");`  
`// error, too few parameters`  
`let result2 = buildName("Bob", "Adams", "Sr.");`  
`// error, too many parameters`
`let result3 = buildName("Bob", "Adams");`   
`// ah, just right`  
在参数名旁使用？实现可选参数的功能。  
`function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}`  
`let result1 = buildName("Bob");  // works correctly now`  
`let result2 = buildName("Bob", "Adams", "Sr.");`  
`// error, too many parameters`  
`let result3 = buildName("Bob", "Adams");`
`// ah, just right`  
**注意：** 可选参数必须放在参数后面。  
在ts中，也可以为参数提供一个默认值当用户没有传递这个参数或者传递的值是undefined时。它叫做有默认初始化值的参数。  
`function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}`  
`let result1 = buildName("Bob");`  
`// works correctly now, returns "Bob Smith"`  
`let result2 = buildName("Bob", undefined);`  
`// still works, also returns "Bob Smith"`  
`let result3 = buildName("Bob", "Adams", "Sr.");`  
`// error, too many parameters`  
`let result4 = buildName("Bob", "Adams");`  
`// ah, just right`  
#### 剩余参数  
必要参数，可选参数，默认参数有共同点，它们表示某一个函数。有时你想同时操作多个参数，并且你不知道多少个参数会被放进来。在js中可以使用arguments来访问所有传入的参数。  
在ts中可以将所有参数收集到一个变量里。  
`function buildName
(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}`  
`let employeeName = 
buildName("Joseph", "Samuel", "Lucas", "MacKinzie");`  
剩余参数会被当作个数不限的可选参数，可以没有，也可以有多个参数。编译器创建参数数组，名字是在...后面给定的名字，可以在函数体内部使用这个数组，这个省略号也会在带有剩余参数的函数类型定义上使用。  
- this和箭头函数  
在js中，this的值在函数被调用的时候才会指定。  
`let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        return function() {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);
            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}`  
<br>`let cardPicker = deck.createCardPicker();`  
`let pickedCard = cardPicker();`  
`alert("card: " + pickedCard.card + " of " + pickedCard.suit);`  
createCardPicker是一个函数，并且又返回了一个函数，运行这个程序会报错，原因是createCardPicker中的this值被设置为window而不是deck对象。  
为了解决这个问题，可以在函数被返回的时候就绑好this。这样无论怎样使用都会引用绑定的deck对象。箭头函数可以保存函数创建时的this，而不是调用的值。    
`let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        // NOTE: the line below is now an arrow function, allowing us to capture 'this' right here
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);
            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}`  
<br>`let cardPicker = deck.createCardPicker();`  
`let pickedCard = cardPicker();`  
`alert("card: " + pickedCard.card + " of " + pickedCard.suit);`  
- this参数  
然而`this.suits[pickedSuit]`的类型依然为any，原因是this来自对象字面量里的函数表达式。修改的方法是，提供一个显示的this参数。this参数是个假的参数，它出现在参数列表的最前面。  
`function f(this: void) {
    // make sure `this` is unusable in this standalone function
}`  
`interface Card {
    suit: string;
    card: number;
}`  
`interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}`  
`let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    // NOTE: The function now explicitly specifies that its callee must be of type Deck
    createCardPicker: function(this: Deck) {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}`  
`let cardPicker = deck.createCardPicker();`  
`let pickedCard = cardPicker();`  
`alert("card: " + pickedCard.card + " of " + pickedCard.suit);`   
此时ts已经知道createCardPicker期望在某个Deck对象上调用。也就是说this是Deck类型的，而非any。  
- this参数在回调函数中  

#### 重载  
js是动态语言。js里的函数根据传入不同的参数而返回不同类型的数据是常见的。  
`let suits = ["hearts", "spades", "clubs", "diamonds"];`  
`function pickCard(x): any {
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}`  
`let myDeck = [
    { suit: "diamonds", card: 2 }, 
    { suit: "spades", card: 10 }, 
    { suit: "hearts", card: 4 }];`  
`let pickedCard1 = myDeck[pickCard(myDeck)];`  
`alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);`  
`let pickedCard2 = pickCard(15);`  
`alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);`  
pickCard方法根据传入的参数不同会返回两种类型，如果传入的是代表纸牌的对象，函数的作用是抓一张牌。  
下面来重载pickCard函数：  
`let suits = ["hearts", "spades", "clubs", "diamonds"];`  
`function pickCard
(x: {suit: string; card: number; }[]): number;`  
`function pickCard
(x: number): {suit: string; card: number; };`  
`function pickCard(x): any {
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}`  
`let myDeck = [
    { suit: "diamonds", card: 2 }, 
    { suit: "spades", card: 10 }, 
    { suit: "hearts", card: 4 }];`  
`let pickedCard1 = myDeck[pickCard(myDeck)];`  
`alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);`  
`let pickedCard2 = pickCard(15);`  
`alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);`  
这样以后，重载的pickCard函数在调用的时候会进行正确的类型检查。  
为了可以让编译器正确的检查类型，它与js的流程相似，它查找重载列表，尝试使用第一个重载定义，如果匹配就使用。因此在定义重载的时候，一定要将最准确的定义放在前面。  

### 6.泛型  
#### 介绍  
软件工程中，不仅要创建一致性良好的API，同时要考虑可重用性。组件不仅能支持当前的数据类型，同时也可以支持未来的数据类型。  
#### 泛型之HelloWord  
创建一个使用泛型的例子identity函数。这个函数会返回任何传入的值。  
不用泛型的话，函数可能会变成下例：  
`function identity(arg: number): number {
    return arg;
}`  
或者使用any类型来定义函数：  
`function identity(arg: any): any {
    return arg;
}`  
使用any类型可以使函数接收任何类型的arg参数，但是这样会丢失一些信息：传入类型与返回的类型应该是一样的，如果传入数字，我们只是知道任何类型的值都会返回。  
因此需要一种方法使返回值的类型与参入的参数的类型是相同的，这里我们使用了类型变量，这是一种特殊的变量，只表示类型不表示值。  
`function identity<T>(arg: T): T { return arg; }`  
给identity添加了类型变量T，T用于捕获用户传入的类型，之后我们就可以使用这个类型，也可以将T类型返回。  
此时这个版本的identity函数叫做泛型，因为它可以适用于多个类型，不同于使用any，它不会丢失信息。  
当定义了泛型函数后，可以用两种方法使用，第一种是传入所有的参数，包含类型参数：
`let output = identity<string>("myString");  // type of output will be 'string'`  
这里明确了T是string类型，并作为一个参数传给函数，使用<>而不是()。  
第二种更加普遍的方式是利用类型推论，即编译器会根据传入的参数自动的帮助我们确定T的类型。  
`let output = identity("myString");  // type of output will be 'string'`    
此时没有必要使用<>来明确传入的类型，编译器可以查看传入参数的值，然后将T设置为它的类型。类型推论帮助我们保持代码精简和高可读性。若编译器不能够自动的判断出类型的话，只能明确传入T的类型。  
#### 使用泛型变量  
`function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  
    // Error: T doesn't have .length
    return arg;
}`  
因为没有指明arg的类型，所以很有可能arg这个类型没有.length方法。  
现在若我们想直接操作T类型的数组，而不是直接操作T，所以：  
`function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  
    // Array has a .length, so no more error
    return arg;
}`  
泛型函数loggingIdentity，接收类型参数T和参数arg，它是个元素类型，是T的数组，并返回元素类型是T的数组。此时可以让我们把泛型变量T当作类型的一部分使用，而不是整个类型，增加了灵活性。  
#### 泛型类型  
创建泛型函数：  
`function identity<T>(arg: T): T {
    return arg;
}`  
`let myIdentity: <T>(arg: T) => T = identity;`  
与普通函数不同的是在有一个类型参数在最前面，像函数声明。  
还可以使用带有调用签名的对象字面量来定义泛型函数。  
`function identity<T>(arg: T): T {
    return arg;
}`  
`let myIdentity: {<T>(arg: T): T} = identity;`  
将上例的对象字面量拿出来作为一个接口。  
`interface GenericIdentityFn {
    <T>(arg: T): T;
}`  
`function identity<T>(arg: T): T {
    return arg;
}`  
`let myIdentity: GenericIdentityFn = identity;`  
<br>
`interface GenericIdentityFn<T> {
    (arg: T): T;
}`  
`function identity<T>(arg: T): T {
    return arg;
}`  
`let myIdentity: GenericIdentityFn<number> = identity;`  
这里不再描述泛型函数，而是把非泛型函数签名作为泛型函数的一部分。当我们使用GenericIdentityFn的时候，还需要传入一个类型参数来指定泛型类型，锁定之后代码里使用的类型。对于描述哪部分类型属于泛型的，理解何时将参数放在调用签名里和何时放在接口上。
除了泛型接口，还可以创建泛型类。  
**注意：** 无法创建泛型枚举和泛型命名空间。  

#### 泛型类  
泛型类与泛型接口相似。泛型类使用<>括起泛型类型，跟在类的后面。  
`class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}`  
`let myGenericNumber = new GenericNumber<number>();`  
`myGenericNumber.zeroValue = 0;`  
`myGenericNumber.add = function(x, y) { return x + y; };`  
没有限制只使用一种类型。  
`let stringNumeric = new GenericNumber<string>();`  
`stringNumeric.zeroValue = "";`  
`stringNumeric.add = function(x, y) { return x + y; };`  
`console.log(stringNumeric.add
(stringNumeric.zeroValue, "test"));`  
与接口一样，直接将泛型类型放在类的后面，可以帮助我们确认类的所有属性都在使用相同的类型。  
类有两部分，静态部分与实例部分。泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。  
#### 泛型约束  
`function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  
    // Error: T doesn't have .length
    return arg;
}`  
相比于操作any所有类型，想要限制函数去处理任意带有.length属性的所有类型，只要传入的类型有这个属性，我们就允许，就是说至少包含这一属性。  
我们定义一个接口描述约束条件。创建一个包含.length属性的接口，使用这个接口和extends关键字来实现约束：  
`interface Lengthwise {
    length: number;
}`  
`function loggingIdentity
<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  
    // Now we know it has a .length property,so no more error
    return arg;
}`  
现在这个泛型函数被定义了约束，因此它不是适用于任何类型：  
`loggingIdentity(3);  
// Error, number doesn't have a .length property`  
需要传入符合约束类型的值，必须包含必须的属性：    
`loggingIdentity({length: 10, value: 3});`  
- 在泛型约束中使用类型参数  
可以声明一个类型参数，且它被另一个类型参数所约束。比如，现在想要用属性名从对象获取这个属性，并且我们想要确保这个属性存在于这个对象obj上，因此需要在这两个类型之间使用约束。  
`function getProperty(obj: T, key: K) {
    return obj[key];
}`  
`let x = { a: 1, b: 2, c: 3, d: 4 };`  
`getProperty(x, "a"); // okay`  
`getProperty(x, "m"); 
// error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.`   
- 在泛型里使用类类型  
在TypeScript使用泛型创建工厂函数时，需要引用构造函数的类类型。  
`function create<T>(c: {new(): T; }): T {
    return new c();
}`  
使用原型属性推断并约束构造函数与类实例的关系。  
`class BeeKeeper {
    hasMask: boolean;
}`  
`class ZooKeeper {
    nametag: string;
}`  
`class Animal {
    numLegs: number;
}`  
`class Bee extends Animal {
    keeper: BeeKeeper;
}`  
`class Lion extends Animal {
    keeper: ZooKeeper;
}`  
`function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}`  
`createInstance(Lion).keeper.nametag;  // typechecks!`  
`createInstance(Bee).keeper.hasMask;   // typechecks!`  

### 枚举  
使用枚举可以定义一些带名字的变量，ts支持数字的和基于字符串的枚举。  
#### 数字枚举  
`enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}`  
以上定义了一个数字枚举，一个成员初始化为1，其余成员将会从1开始自增长。不使用初始化器则第一个成员的值为0，其余自增长。  
使用枚举：通过枚举的属性来访问枚举成员和枚举的名字来访问枚举类型。  
数字枚举可以被混入到计算过的和常量成员。不带初始化器的枚举或者被放在第一位置的或者被放在使用了使用数字常量或其他常量初始化了的美剧后面。
#### 字符串枚举  
在一个字符串枚举中，每个成员都必须使用字符串字面量忙活着另外一个字符串枚举成员进行初始化。  
`enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}`  
由于字符串枚举没有自增长的行为，字符串枚举可以很好的序列化。并且字符串枚举允许你提供一个运行时有意义的并且可读的值，独立于枚举成员的名字。  
#### 异构枚举  
枚举可以混合字符串和数字成员。  
#### 计算的和常量成员  
每个枚举成员都带有一个值，可以是常量或者计算出来的。满足以下条件时，枚举成员被当作常量：  
1. 它是枚举的第一个成员且没有初始化器，这种情况它将被赋值为0。  
2. 它不带有初始化器且它之前的枚举成员是一个数字常量。这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。  
3. 枚举成员使用常量枚举表达式初始化。常数枚举表达式是ts表达式的子集，它可以在编译阶段求值。当一个表达式满足下面条件之一时，它就是一个常量枚举表达式。 
- 一个枚举表达式字面量（主要为字符串字面量或数字字面量）  
- 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的）  
- 带括号的常量枚举表达式。  
- 一元运算符其中之一应用在了常量枚举表达式。  
- 常量枚举表达式作为二元运算符的操作对象。若常数枚举表达式求值后为NaN或Infinity，则会在编译阶段报错。  

其他情况的枚举成员被当作是需要计算得出的值。  
#### 联合枚举与枚举成员类型  
存在一种特殊的非计算的常量枚举成员的子集：字面量枚举成员。字面量枚举成员是指不带有初始值的常量枚举成员，或者值被初始化为：  
- 任何字符串字面量  
- 任何数字字面量  
- 应用了一元 - 符号的数字字面量（-1，-10）  

当所有的枚举成员都拥有字面量枚举值时，那么枚举成员成为了类型，我们可以说某些成员只能是枚举成员的值：  
`enum ShapeKind {
    Circle,
    Square,
}`  
`interface Circle {
    kind: ShapeKind.Circle;
    radius: number;
}`  
`interface Square {
    kind: ShapeKind.Square;
    sideLength: number;
}`  
```
let c: Circle = {
    kind: ShapeKind.Square,
    //    ~~~~~~~~~~~~~~~~ Error!
    radius: 100,
}
```  
另一个变化是
#### 运行时的枚举  
枚举是在运行时真正存在的对象。  
`enum E {
    X, Y, Z
}`  
能够传递给functions。  
`function f(obj: { X: number }) {
    return obj.X;
}`  
`// Works, since 'E' has a property named 'X' which is a number.`  
`f(E);`  
#### 反向映射  
数字枚举成员还具有反向映射，从枚举值到枚举名字。  
`function f(obj: { X: number }) {
    return obj.X;
}`  
`// Works, since 'E' has a property named 'X' which is a number.`  
`f(E);`  
ts会将上面的代码编译成下面的js。  
`var Enum;`  
`(function (Enum) {
    Enum[Enum["A"] = 0] = "A";
})(Enum || (Enum = {}));`  
`var a = Enum.A;`  
`var nameOfA = Enum[a]; // "A"`  
生成的代码中，枚举类型被编译成一个对象，它包含了一个正向映射（name -> value）和反向映射（value -> name）。引用枚举成员总是会生成为对属性访问并且永远也不会内联代码。  
**注意：** 不会为字符串枚举成员生成正向映射。  
#### const枚举  
为了避免在额外的生成的代码上的开销和额外的非直接的对枚举成员的访问，可以使用const枚举。常量枚举通过在枚举上使用const修饰符来定义。  
`const enum Enum {
    A = 1,
    B = A * 2
}`  
常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除，常量的枚举成员在使用的地方会被内联进来，这是因为常量枚举不允许包含计算成员。  
`const enum Directions {
    Up,
    Down,
    Left,
    Right
}`  
`let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right]`  
生成的代码为：  
`var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];`  
#### 外部枚举   
外部枚举用来描述已经存在的枚举类型的形状。  
`declare enum Enum {
    A = 1,
    B,
    C = 2
}`  
外部枚举和非外部枚举有个重要的区别，在正常的枚举中，没有初始化方法的成员会被当作常数成员，对于非常数的外部枚举而言，没有初始化的方法被当作需要经过计算。  
### 类型推论  
### 类型兼容性  
### 高级类型  
### Symbols  
#### 介绍
symbel类型的值是由Symbol构造函数创建的。  
`let sym1 = Symbol();`  
`let sym2 = Symbol("key");//可选的字符串key`  
Symbol是不可改变且唯一的。  
symbols也可以被用作对象属性的键。  
`let sym = Symbol();`  
`let obj = {
    [sym]: "value"
};`  
`console.log(obj[sym]); //value`  
Symbols也可以与计算出的属性名声明相结合来声明对象属性和类成员。  
`const getClassNameSymbol = Symbol();`  
`class C{
    [getClassNameSymbol](){
        return "C";
    }
}`  
`let c = new C();`  
`let className = c[getClassNameSymbol](); //"C"`  
#### 众所周知的Symbols  
除了用户自定义的symbols，内置的symbols用来表示语言内部的行为。  
以下为这些symbols的列表：  

- `Symbol.hasInstance`  
方法，会被instanceof运算符调用，构造器对象用来识别一个对象是否是其实例。  
- `Symbol.isConcatSpreadable`  
布尔值，表示当在一个对象上调用`Array.prototype.concat`时，这个对象的数组元素是否可展开。  
- `Symbol.iterator`  
方法，被`for-of`语句调用，返回对象的默认迭代器。  
- `Symbol.match`  
方法，被`String.prototype.match`调用，正则表达式用来匹配字符串。  
- `Symbol.replace`  
方法，被`String.prototype.replace`调用，正则表达式用来替换字符串中匹配的子串。  
- `Symbol.search` 
方法，被`String.prototype.search`调用，正则表达式返回被匹配部分在字符串中的索引。  
- `Symbol.species`  
函数值，为一个构造函数，用来创建派生对象。  
- `Symbol.split`  
方法，被`String.prototype.split`调用，正则表达式用来分割字符串。  
- `Symbol.toPrimitive`  
方法，被`ToPrimitive`抽象操作调用，把对象转换为相应的原始值。  
- `Symbol.toStringTag`  
方法，被内置方法`Object.prototype.toString`调用，返回创建对象时默认的字符串描述。   
- `Symbol.unscopables`  
对象，它自己拥有的属性会被with作用域排除在外。  
### 迭代器和生成器  
#### 可迭代性 
当一个对象实现了`Symbol.iterator`属性时，我们认为它是可迭代的。一些内置的类型如`Array,Map,Set,String,Int32Array,Unit32Array`等都已经实现了各自的`Symbol.iterator`。对象上的`Symbol.iterator`函数负责返回供迭代的值。  
#### for-of语句  
`for..of`会遍历可迭代的对象，调用对象上的`Symbol.iterator`方法，下面是在数组上使用for-of的简单例子：  
`let someArray = [1, "string", false];`  
`for(let entry of someArray){
    console.log(entry);
    //1, "string", false
}`  
#### for..of与for..in语句  
均表示一个可迭代的列表，但是用于迭代的值不同，for..in迭代是对象的键的列表。而for..of则是迭代对象的键的对应值。  
`let list = [1, 2, 3, 4];`  
`for(let i in list){
    console.log(i);
    //0,1,2,3
}`  
`for(let i of list){
    console.log(i);
    //1,2,3,4
}`  
另一个区别是for..in可以操作任何对象；它提供了查看对象属性的一种方法，但是for..of关注于迭代对象的值，内置对象Map和Set已经实现了Symbol.iterator方法，让我们可以访问它们保存的值。  
`let pets = new Set(["cat", "dog"])`  
`pets["species"] = "mammals";`  
`for(let pet in pets){
    console.log(pet);
    //"species"
}`  
`for(let pet of pets){
    console.log(pet);
    //"cat", "dog"
}`  
#### 代码生成  
目标为es5和es3  
当生成目标为es5和es3时，迭代器只允许在Array类型上使用，在非数组上使用for..of语句会得到一个错误，就算这些非数组值已经实现了Symbol.iterator属性。  
编译器会生成一个简单的for循环做为forof循环，比如：  
`let number = [1,2,3];`  
`for(let num of number){
    console.log(num);
}`  
生成的代码为：  
`var number = [1,2,3];`  
`for(var _i = 0;i < number.length;i++){
    var num = number[_i];
    console.log(num);
}`  
目标为ECMAScript2015或更高时  
编译器会生成相应引擎的for..of内置迭代器实现方式。  
### 模块  
#### 介绍  
从ECMASctipt 2015开始js中引入了模块的概念，ts也沿用这个概念。  
模块在其自身的作用域里执行，而不是在全局作用域中；这意味着定义一个模块里的变量，函数，类等等在模块外部是不可见的，除非你明确的使用export形式之一导出它们，相反如果想要使用其他模块导出的变量，函数，类，接口等的时候，你必须要导入它们，可以使用import形式之一。  
模块是自声明的；两个模块之间的关系是通过在文件级别上使用imports和exports建立的。  
模块使用模块加载器去导入其他的模块，在运行时模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。jsmk加载器是服务于CommonJs和服务于web应用的Require.js。 
ts与ECMAScript 2015一样，任何包含顶级import或者export的文件都被当作成一个模块。相反如果有一个文件不带有顶级的import或者export声明，那么它的内容被视为全局可见的，因此对模块也是可见的。  
#### 导出  
1. 导出声明  
任何声明（变量，函数，类，类型别名或者接口）都能够通过添加export关键字来导出。  
`export interface StringValidator {
    isAcceptable(s: string): boolean;
}`  
`export const numberRegexp = /^[0-9]+$/;`  
`export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}`  
2. 导出语句  
导出语句很便利，因为我们可能需要对导出的部分重命名，所以上面的例子可以改写：  
`class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}`  
`export { ZipCodeValidator };`  
`export { ZipCodeValidator as mainValidator };`  
3. 重新导出  
我们经常会去扩展其他模块，并且只导出那个模块的内容，重新导出功能并不会在当前模块导入那个模块或者定义一个新的局部变量。  
`export class ParseIntBasedZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && parseInt(s).toString() === s;
    }
}`
`// 导出原先的验证器但做了重命名
export {ZipCodeValidator as RegExpBasedZipCodeValidator} from "./ZipCodeValidator";`  
或者一个模块可以包裹多个模块，并把它们导出的内容联合在一起通过语法：  
`export * from "module"`  
`export * from ".StringValidator";   
//exports interface StringValidator`  
`export * from "./LettersOnlyValidator";  
// exports class LettersOnlyValidator`  
`export * from "./ZipCodeValidator";  // exports class ZipCodeValidator`  
#### 导入  
1. 导入一个模块中的某个内容  
`import { ZipCodeValidator } from "./ZipCodeValidator";`  
`let myValidator = new ZipCodeValidator();`  
2. 对导入的内容重命名  
`import { ZipCodeValidator as ZCV } from "./ZipCodeValidator";`  
`let myValidator = new ZCV();`  
3. 将整个模块导入到一个变量，并通过它来访问模块的导出部分。  
`import * as validator from "./ZipCodeValidator";`  
`let myValidator = new validator.ZipCodeValidator();`  
4. 具有副作用的导入模块  
不推荐，一些模块会设置一些全局状态供其他模块使用。这些模块可能没有任何的导出或用户根本就不关注它的导出：  
`import "./my-module.js";`  
#### 默认导出   
每个模块都可以有一个默认导出。默认导出使用default关键字标记；并且一个模块只能有一个默认导出，需要使用一种特殊的导入形式来导入default导出。  
default导出很便利，像JQ这样的类库可能有一个默认的导出JQuery或$,并且也会使用同样的名字JQuery或$导出JQuery。  
导出：
`declare let $: JQuery;`  
`export default $;`  
导入：
`import $ from "JQuery";`   
`$("button.continue").html( "Next Step..." );`  
类和函数声明可以直接被标记为默认导出。标记为默认导出的类和函数的名字是可以省略的。  
#### `exoprt`和`import = require()`  
CommonJs和AMD的环境里都有一个exports变量，这个变量包含了一个模块的所有导出内容。  
CommonJs和AMD的exports都可以被赋值为一个对象，这种情况下其作用域就类似于es6语法里的默认导出，即export default语法，虽然作用类似，但是export default语法并不能兼容CommonJs和AMD的exports。 
为了支持CommonJs和AMD的exports，ts提供了export = 语法。  
export = 语法定义一个模块的导出对象，这里的对象一词指的是类，接口，命名空间，函数或枚举。  
若使用export = 导出一个模块，则必须使用ts的特定语法，import module = require("module")来导入模块。  
#### 生成模块代码  
根据编译时指定的模块目标参数，编译器会生成相应的供各个模块加载系统使用的代码。  

#### 可选的模块加载和其他高级场景   

### 命名空间
#### 第一步  
定于几个简单的字符串验证器，假设会用来验证表单里的用户输入或验证外部数据。  
**所有的验证器都放在一个文件里**  
```
interface StringValidator {
    isAcceptable(s: string): boolean;
}

let lettersRegexp = /^[A-Za-z]+$/;
let numberRegexp = /^[0-9]+$/;

class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
        return lettersRegexp.test(s);
    }
}

class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: StringValidator; } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters only"] = new LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        let isMatch = validators[name].isAcceptable(s);
        console.log(`'${ s }' ${ isMatch ? "matches" : "does not match" } '${ name }'.`);
    }
}
```  
#### 命名空间  
随着验证器的加入，我们需要一种手段来组织代码，以便于记录它们类型的同时还不用担心与其他对象产生命名冲突。因此可以将验证器包裹到一个命名空间内，而不是把它们放在全局命名空间下。  
下例中，将所有与验证器相关的类型都放到Validation的命名空间内，因为想让这些接口和类在命名空间之外也可以访问，苏哦一需要export，相反的，变量lettersRegexp和numberRegexp是实现的细节，不需要导出，因此它们在命名空间之外是不可以访问的，在文件末尾的测试代码中，由于是在命名空间之外访问，因此需要限定类型的名称。  
**使用命名空间的验证器**  
```
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```
#### 分离到多文件  
当应用变得越来越大，我们需要将代码分离到不同的文件中以便于维护。  
**多文件中的命名空间**  
将Validation命名空间分割成多个文件。尽管是不同的文件，它们仍是同一个命名空间，并且在使用的时候如同它们在一个文件中定义的一样。因为不同的文件之间存在依赖关系，所以我们加入了引用标签来告诉编译器之间的关联，测试代码不变。  
### 命名空间和模块  
### 模块解析  
### 声明合并  
### JSX  
### 装饰器  
### Mixins  
### 三斜线指令  
### js文件检查