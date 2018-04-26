# js基础概念

## ES6常用新特性

### let和块级作用域

#### let变量的特性
- let没有`变量提升`，必须先声明再使用，否则报错（而不是`undefined`）。
- 不能重复声明（而不是最后声明的值被赋值成功）。

#### 块级作用域
let变量只在当前代码块内可以访问，比如一个循环或者一个函数等都是一个代码块。或者说，`成对花括号内`部即为一个代码块。

let关键字可以取代以前用“立即执行函数”来模拟块级作用域的场景：

```js
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

```js
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

### const变量

用于声明只读的变量。所以必须在声明的时候进行初始化。先声明后使用，不可重复声明。

**深入理解**
const实际是保证变量地址不变，所以在简单类型上可以保证值不变，但是在引用类型上，情况有些不一样。对应const声明的引用类型，实际上是保证了指针的地址，所以可以改变属性值，但是不能重新赋值。

备注：使用`Object.freeze`方法可以使对象属性也不可更改。
备注2：如果被冻结的对象中包含另一个对象，需要在该对象上面再次执行`freeze`。下面是一个将对象彻底冻结的函数。

```js
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

#### 认识顶级对象

- 浏览器环境：window、self
- web worker环境：self
- node环境：global

ES6 为了改变**顶层对象的属性与全局变量挂钩**这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩

### 解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

**数组的解构**：直接按位置、次序解构

**对象的解构**：对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```js
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```

上面代码中，foo是匹配的模式，baz才是变量。真正被赋值的是变量baz，而不是模式foo。

#### 对象赋值简写

ES6 允许直接写入变量和函数，作为对象的属性和方法。

```js
var x = "a";
var y = "b";
var v = {x,y}
// 等同于
var v = {x: x, y: y}; //{a:a,b:b}
```

#### 默认值
注意，ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员或对象d 属性值严格等于undefined，默认值才会生效。

`...`扩展运算符，可以简化数组数值，简化函数传参。

```js
//数组默认值
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError: y is not defined
//对象默认值
var {x = 3} = {};
x // 3
var {x, y = 5} = {x: 1};
x // 1
y // 5
var {x: y = 3} = {};
y // 3
var {x: y = 3} = {x: 5};
y // 5
var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"

var {x = 3} = {x: undefined};
x // 3
var {x = 3} = {x: null};
x // null

```

#### 函数传参的应用
解构赋值应用在函数传参：
数目不定的入参
入参默认值,不用在函数体内用`|`做显式声明
入参直接当做数组用（rest参数，纯...模式）

```js
function fn1([a, b, c]) {}
function fn1(...arr) {} //arr = [a,b,c]
function fn1(a,...arr) {} //arr = [b,c]
function fn1(a=1,b=2,c=3) {}
```

### 箭头函数

ES6 允许使用“箭头”（=>）定义函数。

箭头函数体可以是表达式、变量（对象需要加一层圆括号）、代码块、或者无返回值。
使用示例：
```js
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};

//无返回值
let fn = () => void doesNotReturn();

//回调
// 正常函数写法
[1,2,3].map(function (x) {
  return x * x;
});
// 箭头函数写法
[1,2,3].map(x => x * x);

//解构
const full = ({ first, last }) => first + ' ' + last;
// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}

//rest参数
const numbers = (...nums) => nums;
numbers(1, 2, 3, 4, 5) // [1,2,3,4,5]
const headAndTail = (head, ...tail) => [head, tail];
headAndTail(1, 2, 3, 4, 5) // [1,[2,3,4,5]]
```

箭头函数有几个使用注意点：

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

箭头函数可以让this指向固定化。是指箭头函数里面的this，绑定定义时所在的作用域，而不是指向运行时所在的作用域。实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数。

```js
// ES6
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

// ES5
function foo() {
  var _this = this;

  setTimeout(function () {
    console.log('id:', _this.id);
  }, 100);
}
```

所以对箭头函数应用`call`、`apply`、`bind`达不到预期的效果。

箭头函数也没有`arguments`、`super`、`new.target`

### 模板字符串

模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```js
// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

所有的空格和缩进都会被保留在输出之中
${}内部可以是变量、表达式、对象引用、函数等。非字符串内容会执行`toString`方法转换。

### Iterators（迭代器）+for..of

Iterator 的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是 ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供for...of消费。

```js
// 去除数组的重复成员
[...new Set(array)]
```

部署了`Symbol.iterator`属性的数据结构，就称为部署了遍历器接口。原生带遍历器：Array、Map、Set、String、TypedArray、函数的 arguments 对象、NodeList 对象。**对象**原生不带遍历器。

解构赋值、扩展运算符等都使用的遍历器接口。

可以使用如下类似方法手动添加`Symbol.iterator`属性。

```js
let obj = {
  data: [ 'hello', 'world' ],
  [Symbol.iterator]() {
    const self = this;
    let index = 0;
    return {
      next() {
        if (index < self.data.length) {
          return {
            value: self.data[index++],
            done: false
          };
        } else {
          return { value: undefined, done: true };
        }
      }
    };
  }
};
```

属性名Symbol.iterator，它是一个表达式，返回Symbol对象的iterator属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内。

for...of循环内部调用的是数据结构的Symbol.iterator方法

#### 覆盖默认的Iterator
```js
var str = new String("hi");
[...str] // ["h", "i"]
str[Symbol.iterator] = function() {
  return {
    next: function() {
      if (this._first) {
        this._first = false;
        return { value: "bye", done: false };
      } else {
        return { done: true };
      }
    },
    _first: true
  };
};
[...str] // ["bye"]
str // "hi"
```

#### 遍历器对象

实现遍历器对象时next方法是必须部署的，return方法和throw方法是否部署是可选的。

**return方法**：使用场合是，如果for...of循环提前退出（通常是因为出错，或者有break语句或continue语句），就会调用return方法。如果一个对象在完成遍历前，需要清理或释放资源，就可以部署return方法。

```js
function readLinesSync(file) {
  return {
    [Symbol.iterator]() {
      return {
        next() {
          return { done: false };
        },
        return() {
          file.close();
          return { done: true };
        }
      };
    },
  };
}

// 情况一，输出文件的第一行以后，就会执行return方法，关闭这个文件
for (let line of readLinesSync(fileName)) {
  console.log(line);
  break;
}

// 情况二，输出所有行以后，执行return方法，关闭该文件
for (let line of readLinesSync(fileName)) {
  console.log(line);
  continue;
}

// 情况三，执行return方法关闭文件之后，再抛出错误
for (let line of readLinesSync(fileName)) {
  console.log(line);
  throw new Error();
}
```

**throw方法**：主要是配合 Generator 函数使用，一般的遍历器对象用不到这个方法。

#### entries()、keys()、values()

ES6 的数组、Set、Map 都部署了以下三个方法，调用后都返回遍历器对象。

entries() 返回一个遍历器对象，用来遍历[键名, 键值]组成的数组。对于数组，键名就是索引值；对于 Set，键名与键值相同。Map 结构的 Iterator 接口，默认就是调用entries方法。

keys() 返回一个遍历器对象，用来遍历所有的键名。

values() 返回一个遍历器对象，用来遍历所有的键值。

```js
let arr = ['a', 'b', 'c'];
for (let pair of arr.entries()) {
  console.log(pair);
}
// [0, 'a']
// [1, 'b']
// [2, 'c']
```

### Generator 函数

```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```
上面代码定义了一个 Generator 函数helloWorldGenerator，它内部有两个`yield表达式`（hello和world），即该函数有三个状态：hello，world 和 return 语句（结束执行）。

调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是遍历器对象（Iterator Object）。该对象本身也具有`Symbol.iterator`属性，执行后返回自身。————所以，可以直接使用Generator函数赋值给`Symbol.iterator`来使对象具有 Iterator 接口

必须调用遍历器对象的next方法，使得指针移向下一个状态。

`*号前后有无空格均可`

遍历器对象的`next方法的运行逻辑`如下。

（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

#### next参数

yield表达式本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作`上一个yield表达式的返回值`。

可以在 Generator 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。

```js
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}
var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}
var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

#### throw方法
Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

Generator 函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获。

```js
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log(e);
  }
};

var i = g();
i.next();
i.throw(new Error('出错了！'));
// Error: 出错了！(…)
try {
  i.throw('a');
} catch (e) {
  console.log('外部捕获', e);
}
// 外部捕获 a
// 内部catch语句已经执行过了，不再执行。
```
如果 Generator 函数内部和外部，都没有部署try...catch代码块，那么程序将报错，直接中断执行。

throw方法被捕获以后，会附带执行下一条yield表达式。也就是说，会附带执行一次next方法。

`全局throw命令抛出的错误，只能被函数体外的catch语句捕获。`

throw命令与g.throw方法是无关的，两者互不影响

```js
var gen = function* gen(){
  yield console.log('hello');
  yield console.log('world');
}

var g = gen();
g.next();

try {
  throw new Error();
} catch (e) {
  g.next();
}
// hello
// world
```

### Class

声明类的关键字，属性用法与`prototype`关键字一致

```js
//ES5写法
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
//ES6写法
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
//不需要逗号
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

class与es5有几个主要的区别

1.使用这种方式声明的类方法是不可枚举的，如下

```js
var Point = function (x, y) {
  // ...
};
Point.prototype.toString = function() {
  // ...
};
Object.keys(Point.prototype)
// ["toString"],采用 ES5 的写法，toString方法就是可枚举的
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

2.类必须使用new调用

```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}
Foo()
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

3.不存在变量提升，必须先定义再使用
```js
new Foo(); // ReferenceError
class Foo {}
```

类方法里面的this默认指向实例，需要在实例上面调用（而不是类直接调用）。

静态方法使用类直接调用。

### 继承

使用`extends `关键字进行类的继承

```js
class Point {
}
class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }
  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。必须先调super，再使用this。

不显示添加也会有默认的`constructor`

```js
constructor(...args) {
  super(...args);
}
```

父类的静态方法，也会被子类继承

```js
class A {
  static hello() {
    console.log('hello world');
  }
}
class B extends A {
}
B.hello()  // hello world
```

#### Object.getPrototypeOf
```js
Object.getPrototypeOf(ColorPoint) === Point // true
```
Object.getPrototypeOf方法可以用来从子类上获取父类。因此，可以使用这个方法判断，一个类是否继承了另一个类。

#### super
* 作为函数。指向父类的构造函数
* 作为对象。在普通方法中，指向父类的原型对象（取不到父类实例上的方法或属性）；在静态方法中，指向父类。


### Module

ES6 模块的设计思想是尽量的静态化，使得编译时（对比CommonJS等的运行时加载）就能确定模块的依赖关系，以及输入和输出的变量。

ES6 模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入。

#### 默认严格模式

ES6 的模块自动采用严格模式。严格模式主要有以下限制。

* 变量必须声明后再使用
* 函数的参数不能有同名属性，否则报错
* 不能使用with语句
* 不能对只读属性赋值，否则报错
* 不能使用前缀 0 表示八进制数，否则报错
* 不能删除不可删除的属性，否则报错
* 不能删除变量delete prop，会报错，只能删除属性delete global[prop]
* eval不会在它的外层作用域引入变量
* eval和arguments不能被重新赋值
* arguments不会自动反映函数参数的变化
* 不能使用arguments.callee
* 不能使用arguments.caller
* 禁止this指向全局对象
* 不能使用fn.caller和fn.arguments获取函数调用的堆栈
* 增加了保留字（比如protected、static和interface）

#### export命令

export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。

```js
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
export {firstName, lastName, year};
```

模块（js文件）外部无法使用没有被export的变量、函数或类。

`重命名`：通常情况下，export输出的变量就是本来的名字，但是可以使用as关键字重命名。

```js
function v1() { ... }
function v2() { ... }
export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

`export的内容必须是接口`：与内部变量有对应关系。
```js
// 报错
export 1;
// 报错
var m = 1;
export m;
// 写法一
export var m = 1;
// 写法二
var m = 1;
export {m};
// 写法三
var n = 1;
export {n as m};
// 报错
function f() {}
export f;
// 正确
export function f() {};
// 正确
function f() {}
export {f};
```

`动态绑定`：export语句输出的接口，可以取到模块内部实时的值。

export命令可以出现在模块的任何位置，只要处于模块顶层就可以。

#### import命令
import命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（profile.js）对外接口的`名称相同`。

其中.js后缀可以省略，相对路径绝对路径都行。不带路径的时候需要有对应的配置文件指定模块的位置。
```js
// main.js
import {firstName as fname, lastName, year} from './profile.js';
function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}
```

* import的内容是只读的。
* import语句会提升到头部执行。
* import是静态执行，所以不能使用表达式和变量。
* 重复的import同一个模块只会引入一个实例，对应多个名字。
* 不要在一个文件中混用CommonJS 模块的require命令和 ES6 模块的import命令

#### 整体加载
模块有多个接口时，可以使用`*`号整体加载。
```js
//另有一个circle.js文件，它输出两个方法area和circumference。
import * as circle from './circle';
console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));
// 下面两行都是不允许的
circle.foo = 'hello';
circle.area = function () {};
```

#### export default
使用export default来指定默认输出接口。此时import命令可以使用任意名字，且不用花括号。
```js
// 第一组
export default function crc32() { // 输出
  // ...
}
import crc32 from 'crc32'; // 输入
// 第二组
export function crc32() { // 输出
  // ...
};
import {crc32} from 'crc32'; // 输入
```

export default命令的本质是将后面的值，赋给`default`变量，即`import default`是允许的。

一条import语句中，可以同时输入默认方法和其他接口。

```js
import { default as foo } from 'modules';
import _, { each, each as forEach } from 'lodash';
```

#### 组合使用export和import
export和import可以在同一个文件中，但是如果写在同一行那么本文件就无法使用该模块，被视为是接口的转发。
```js
export { foo, bar } from 'my_module';
// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };

// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';

export { default } from 'foo';

export { es6 as default } from './someModule';
// 等同于
import { es6 } from './someModule';
export default es6;

export { default as es6 } from './someModule';
```

#### 模块继承
使用前述`export * from 'parent_module';`，然后加上自己的接口即可视为继承。

注意：`export *`会忽视掉default接口。

#### 常量管理
使用`接口转发`机制实现跨模块的常量管理。
```js
// constants/db.js
export const db = {
  url: 'http://my.couchdbserver.local:5984',
  admin_username: 'admin',
  admin_password: 'admin password'
};
// constants/user.js
export const users = ['root', 'admin', 'staff', 'ceo', 'chief', 'moderator'];

// constants/index.js
export {db} from './db';
export {users} from './users';

// script.js
import {db, users} from './index';
```