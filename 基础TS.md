# TypeScript入门
## 环境搭建
* 使用npm：`npm install -g typescript`。
* 安装Visual Studio的TypeScript插件。

## 编译
使用`tsc greeter.ts`将.ts文件编译为.js文件运行。

## 类型注解
TS增加了一种轻量的变量类型约束方式，在变量名后面加上`:type`来实现。
```js
function greeter(person: string) {
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