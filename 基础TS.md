# TypeScript入门
## 环境搭建
* 使用npm：`npm install -g typescript`。
* 安装Visual Studio的TypeScript插件。

## 编译
使用`tsc greeter.ts`将.ts文件编译为.js文件运行。

## 类型注解
TS增加了一种轻量的变量类型约束方式，在变量名后面加上`:type`来实现。
```js
// 括号后面的注解表示函数返回值类型
function greeter(person: string):string {
    return "Hello, " + person;
}
```
收到预期之外的类型时会产生报错，TS提供了静态代码分析来帮助分析代码结构和提供的类型注解。

注意：TS仅做出警告，js文件仍然能够编译生成，只是ts不确保其运行效果。

## 接口
保证解构即可，不用显式使用`implements`语句。
```js
interface Person {
    firstName: string;
    lastName: string;
}
function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
let user = { firstName: "Jane", lastName: "User" };
document.body.innerHTML = greeter(user);
```

## 类
TypeScript支持JavaScript的新特性，比如支持基于类的面向对象编程。

在构造函数的参数上使用public等同于创建了同名的成员变量。

注意类和接口可以一起工作。
```js
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}
interface Person {
    firstName: string;
    lastName: string;
}
function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
let user = new Student("Jane", "M.", "User");
document.body.innerHTML = greeter(user);
```

## 泛型
可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。
```ts
//指定了number，如果要适配string的需要重新定义另一个函数
function identity(arg: number): number {return arg; }
// 可以使用任意类型入参，但是无法保证出参类型与入参相同
function identity(arg: any): any {return arg;}

// 既可以适配各种入参类型，也保证了出参类型
function identity<T>(arg: T): T {
    return arg;
}
```
调用`泛型函数`的时候，可以指定类型如`identity<string>("myString")`，也可以不指定类型由编译器自动识别（类型推论）如`identity("myString")`。类型推论帮助我们保持代码精简和高可读性。如果编译器不能够自动地推断出类型的话，只能像上面那样明确的传入T的类型，在一些复杂的情况下，这是可能出现的。 

