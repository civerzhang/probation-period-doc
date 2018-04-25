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
上面代码定义了一个 Generator 函数helloWorldGenerator，它内部有两个yield表达式（hello和world），即该函数有三个状态：hello，world 和 return 语句（结束执行）。

调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象

必须调用遍历器对象的next方法，使得指针移向下一个状态。

`*号前后有无空格均可`

遍历器对象的`next方法的运行逻辑`如下。

（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。