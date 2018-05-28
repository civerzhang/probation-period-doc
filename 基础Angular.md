# 壳组件
使用angular-cli的`ng new`命令生成的工程，自带一个`appcomponent`组件，称为壳组件，是本工程的顶级组件。

各个`元数据`的组织方式被放在 `@Component` 装饰器或`@NgModule` 装饰器中。其中前者针对各个组件，由组件维护，或者是顶级类，维护整体公共所需元数据。

各个子组件、服务要在`@NgModule`中声明（`declarations`），服务要在提供商（`providers`），外部模块要在引入（`imports`）。示例：
```ts
@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent,
    HeroDetailComponent,
    MessagesComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    APPRoutingModule
  ],
  providers: [
    HeroService,
    MessageService
  ],
  bootstrap: [ AppComponent ]
})
```

# 绑定，[]和()和{}

* 单向从数据源到视图: 
  * 语法：`{{expression}}、[target]"expression"、bind-target="expression"`
  * 场景：插值表达式、属性Property、Attribute、CSS 类、样式
* 从视图到数据源的单向绑定：
  * 语法`(target)="statement"、on-target="statement"`
  * 场景：事件
* 双向
  * 语法：`[(target)]="expression"、bindon-target="expression"`
  * 场景：双向

## 附1：expression和statement
### 表达式
JavaScript 中那些具有或可能引发副作用的表达式是被禁止的，包括：
* 赋值 (=, +=, -=, ...)
* new 运算符
* 使用 ; 或 , 的链式表达式
* 自增和自减运算符：++ 和 --
和 JavaScript 语法的其它显著不同包括：
* 不支持位运算 | 和 &
* 具有新的模板表达式运算符，比如 |、?. 和 !。
### 语句
模板语句解析器和模板表达式解析器有所不同，特别之处在于它`支持基本赋值(=)和表达式链(;和,)`。然而，某些JavaScript语法仍然是不允许的：
* new 运算符
* 自增和自减运算符：++ 和 --
* 操作并赋值，例如 += 和 -=
* 位操作符 | 和 &
* 模板表达式运算符

## 附2：HTML attribute 与 DOM property 的对比

* attribute 是由 HTML 定义的。property 是由 DOM (Document Object Model) 定义的。
* 少量 HTML attribute 和 property 之间有着 1:1 的映射，如 id。
* 有些 HTML attribute 没有对应的 property，如 colspan。
* 有些 DOM property 没有对应的 attribute，如 textContent。
* 大量 HTML attribute 看起来映射到了 property…… 但却不像你想的那样！

attribute 初始化 DOM property，然后它们的任务就完成了。property 的值可以改变；attribute 的值不能改变。

HTML 的 value 这个 attribute 指定了初始值；DOM 的 value 这个 property 是当前值。

`模板绑定是通过 property 和事件来工作的，而不是 attribute。`

```html
<!-- 实在需要使用attribute的时候可以这样绑定: -->
<table border=1>
  <!-- expression calculates colspan=2 -->
  <tr><td [attr.colspan]="1 + 1">One-Two</td></tr>
  <!-- ERROR: There is no `colspan` property to set!
  <tr><td colspan="{{1 + 1}}">Three-Four</td></tr>
  -->
</table>
 <!-- 绑定class -->
<!-- reset/override all class names with a binding  -->
<div class="bad curly special" [class]="badCurly">Bad curly</div>
<!-- toggle the "special" class on/off with a property -->
<div [class.special]="isSpecial">The class binding is special</div>
<!-- binding to `class.special` trumps the class attribute -->
<div class="special" [class.special]="!isSpecial">This one is not so special</div>
<!-- 内联样式 -->
<button [style.background-color]="canSave ? 'cyan': 'grey'" >Save</button>
<button [style.font-size.%]="!isSpecial ? 150 : 50" >Small</button>
<button [style.fontSize.em]="isSpecial ? 3 : 1" >Big</button>
```

**在 Angular 的世界中，attribute 唯一的作用是用来初始化元素和指令的状态。 当进行数据绑定时，只是在与元素和指令的 property 和事件打交道，而 attribute 就完全靠边站了。**

# ngclass、ngstyle
把 ngClass 绑定到一个 key:value 形式的控制对象。这个对象中的每个 key 都是一个 CSS 类名，如果它的 value 是 true，这个类就会被加上，否则就会被移除。
```html
<div [ngClass]="currentClasses">
This div is initially saveable, unchanged, and special</div>
<div [ngStyle]="currentStyles">
This div is initially italic, normal weight, and extra large (24px).</div>
```
```ts
currentClasses: {};
setCurrentClasses() {
  // CSS classes: added/removed per current state of component properties
  this.currentClasses =  {
    'saveable': this.canSave,
    'modified': !this.isUnchanged,
    'special':  this.isSpecial
  };
}
currentStyles: {};
setCurrentStyles() {
  // CSS styles: set per current state of component properties
  this.currentStyles = {
    'font-style':  this.canSave      ? 'italic' : 'normal',
    'font-weight': !this.isUnchanged ? 'bold'   : 'normal',
    'font-size':   this.isSpecial    ? '24px'   : '12px'
  };
}
```


# @input装饰器

父组件向子组件传值，父组件以`属性绑定`形式传递，子组件类接受数据需要加上`@input`装饰器来识别数据，才能在组件中渲染出来。
```ts
//父组件
<subComponent [variableName]='someData'></subComponent>

//子组件
import { Component, OnInit, Input } from '@angular/core';
@Input() variableName;
```

# 服务，依赖注入
组件对应视图，服务对应数据。
数据 -> 服务 -> 组件 -> 视图

组件不应该直接获取或保存数据，它们不应该了解是否在展示假数据。 它们应该聚焦于展示数据，而把数据访问的职责委托给某个服务。

Service 可以从任何地方获取数据：Web 服务、本地存储（LocalStorage）或一个模拟的数据源。

服务是在多个“互相不知道”的类之间共享信息的好办法。（因为父组件注入的服务子组件可以使用。）

从组件中移除数据访问逻辑，意味着将来任何时候你都可以改变目前的实现方式，而不用改动任何组件。 这些组件不需要了解该服务的内部实现。

服务不被注入的时候，只是一个普通的类。下面是两种注入语法。

```ts
providers: [
  UserService,
  //等同于
  { provide: UserService, useValue: UserService }
],
```

可以在module或者component下注入，他们在**有效范围和时间**上有所区别：
* 使用**module**注入：注册在应用的根注入器上，可以往它所创建的任何类中注入相应的服务；一旦创建，服务的实例就会存在于该应用的全部生存期中，Angular 会把这一个服务实例注入到需求它的每个类中。
* 使用**component**注入：注册到每个组件实例自己的注入器上。只能在该组件及其各级`子组件`的实例上注入这个服务实例，而不能在其它地方注入这个服务实例。当组件实例被销毁的时候，服务的实例也同样会被销毁。

# 路由
## 基本路由
```ts
{ path: 'heroes', component: HeroesComponent }
```
## 重定向路由
```ts
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },
```
## 参数路由
```ts
{ path: 'detail/:id', component: HeroDetailComponent },
```
使用`routerLink`进行路由跳转。
```ts
<a *ngFor="let hero of heroes" class="col-1-4" routerLink="/detail/{{hero.id}}">
```
`ActivatedRoute` 保存着到这个 HeroDetailComponent 实例的路由信息。 这个组件对从 URL 中提取的路由参数感兴趣。 其中的 id 参数就是要现实的英雄的 id。

`HeroService` 从远端服务器获取英雄数据，本组件将使用它来获取要显示的英雄。

`location` 是一个 Angular 的服务，用来与浏览器打交道。 稍后，你就会使用它来导航回上一个视图。

```ts
const id = +this.route.snapshot.paramMap.get('id');
```
`route.snapshot` 是一个路由信息的静态快照，抓取自组件刚刚创建完毕之后。

`paramMap` 是一个从 URL 中提取的路由参数值的字典。 "id" 对应的值就是要获取的英雄的 id。

路由参数总会是字符串。 JavaScript 的 `(+) 操作符`会把字符串转换成数字，英雄的 id 就是数字类型。

# 变化检测
为了使数据变化能够实时在视图上得到反馈，引入变化检测机制。

Angular与AngularJS都采用脏检查的变化检测机制，前者优于后者主要体现在：
* 单向数据流向
* 以组件为单位维度独立进行
* 生产环境只进行一次检查
* 可自定义的变化检测策略： Default和onPush
* 可自定义的变化检测操作：markForcheck()，detectChanges()，detach()， reattach()，checkNoChanges()代码实现上的优化，据说采用了VM friendly的代码（这点我也不太明白，就随便提一下）

## Zones
使用zone拦截和跟踪异步工作。Zone 是一个全局的对象，用来配置有关如何拦截和跟踪异步回调的规则。Zone 有以下能力：
* 拦截异步任务调度
* 提供了将数据附加到 zones 的方法
* 为异常处理函数提供正确的上下文
* 拦截阻塞的方法，如 alert、confirm 方法
Zone 采用猴子补丁 (Monkey-patched) 的方式，将 JavaScript 中的异步任务都进行了包装，这使得这些异步任务都能运行在 Zone 的执行上下文中，每个异步任务在 Zone 中都是一个任务，除了提供了一些供开发者使用的钩子外，默认情况下 Zone 重写了以下方法：
* setInterval、clearInterval、setTimeout、clearTimeout
* alert、prompt、confirm
* requestAnimationFrame、cancelAnimationFrame
* addEventListener、removeEventListener

## 变化检测器
angular每个组件都有自己的`变化检测器`，整个应用程序也是一个变化检测器。它负责检测到更新，然后通知视图刷新。引起变化的主要有三类（都是异步操作）：
* Events：click, mouseover, keyup ...
* Timers：setInterval、setTimeout
* XHRs：Ajax(GET、POST ...)

变化检测是单向的，变化总是从根组件开始。所以对于单次变化，只需要检测一次。

当输入属性变化的时候，可以通过组件提供的生命周期钩子 `ngOnChanges` 捕获到变化的内容，即 changes 对象，该对象的内部结构是 key-value 键值对的形式，其中 key 是输入属性的值，value 是一个 SimpleChange 对象，该对象内包含了 previousValue (之前的值) 和 currentValue (当前值)。需要注意的是，如果在组件内手动改变输入属性的值，ngOnChanges 钩子是不会触发的。

## 变化检测策略配置
```ts
@Component({
    selector: '',
    template: ``,
    changeDetection: ChangeDetectionStrategy.OnPush
})
```

OnPush内部使用`===`来校验新值与旧值是否相等，所以使用 OnPush 策略时，需要使用的 Immutable 的数据结构，才能保证程序正常运行。Immutable 即不可变，表示当数据模型发生变化的时候，我们不会修改原有的数据模型，而是创建一个新的数据模型。

除了使用angular提供的默认检测和onpush检测，也可以用 `Observable` 与 `ChangeDetectorRef` 对象提供的 API，来手动控制组件的变化检测行为。

`ChangeDetectorRef` 是组件的变化检测器的引用。
```ts
import { ChangeDetectorRef } from '@angular/core';
@Component({}) class MyComponent {
    constructor(private cdRef: ChangeDetectorRef) {}
}
```
主要的方法有：
```ts
export abstract class ChangeDetectorRef {
  //在组件的 metadata 中如果设置了 changeDetection: ChangeDetectionStrategy.OnPush 条件，那么变化检测不会再次执行，除非手动调用该方法。
  abstract markForCheck(): void;
  //从变化检测树中分离变化检测器，该组件的变化检测器将不再执行变化检测，除非手动调用 reattach() 方法。
  abstract detach(): void;
  //重新添加已分离的变化检测器，使得该组件及其子组件都能执行变化检测
  abstract reattach(): void;
  //从该组件到各个子组件执行一次变化检测
  abstract detectChanges(): void;
}
```

使用 `Observables` 机制提升性能和不可变的对象类似，但当发生变化的时候，Observables 不会创建新的模型，但我们可以通过订阅 Observables 对象，在变化发生之后，进行视图更新。使用 Observables 机制的时候，我们同样需要设置组件的变化检测策略为 OnPush。

`Observables` 示例
counter.component.ts
```ts
import { Component, Input, OnInit, ChangeDetectionStrategy, 
         ChangeDetectorRef } from '@angular/core';
import { Observable } from 'rxjs/Rx';

@Component({
    selector: 'exe-counter',
    template: `
      <p>当前值: {{ counter }}</p>
    `,
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class CounterComponent implements OnInit {
    counter: number = 0;

    @Input() addStream: Observable<any>;

    constructor(private cdRef: ChangeDetectorRef) { }

    ngOnInit() {
        this.addStream.subscribe(() => {
            this.counter++;
            this.cdRef.markForCheck();
        });
    }
}
```
app.component.ts
```ts
import { Component, OnInit } from '@angular/core';
import { Observable } from 'rxjs/Rx';

@Component({
  selector: 'exe-app',
  template: `
   <exe-counter [addStream]='counterStream'></exe-counter>
  `
})
export class AppComponent implements OnInit {
  counterStream: Observable<any>;
  
  ngOnInit() {
     this.counterStream = Observable.timer(0, 1000); 
  }
}
```

## 观察者模式（发布订阅）
### RxJS Subject
Subject有以下特点：
* Subject 既是 Observable 对象，又是 Observer 对象
* 当有新消息时，Subject 会对内部的 observers 列表进行组播 (multicast)

`Subject`使用遍历的形式进行信息发布，中途有某个观察者出错导致遍历中断会使后面的观察者都无法接收到新的消息。为了解决这个问题，需要在增加观察者时给每个观察者添加错误处理，示例：
```ts
const source = Rx.Observable.interval(1000);
const subject = new Rx.Subject();

const example = subject.map(x => {
    if (x === 1) {
        throw new Error('oops');
    }
    return x;
});

subject.subscribe(
    x => console.log('A', x),
    error => console.log('A Error:' + error)
);
    
example.subscribe(
    x => console.log('B', x),
    error => console.log('B Error:' + error)
);

subject.subscribe(
    x => console.log('C', x),
    error => console.log('C Error:' + error)
);

source.subscribe(subject);
```

`Subject`的五个常用方法：
* next - 每当 Subject 对象接收到新值的时候，next 方法会被调用
* error - 运行中出现异常，error 方法会被调用
* complete - Subject 订阅的 Observable 对象结束后，complete 方法会被调用
* subscribe - 添加观察者
* unsubscribe - 取消订阅 (设置终止标识符、清空观察者列表)

### BehaviorSubject
`BehaviorSubject`是Subject的子类，相比于Subject，有以下不同：
* 保存最新的值。
* 添加新的观察者的时候会自动给新观察者发送一次最新的值。
使用的时候需要设置初始值：
```ts
var subject = new Rx.BehaviorSubject(0); // 设定初始值
```
### ReplaySubject
`ReplaySubject`是Subject的子类，相比于Subject，有以下不同：
* 有自己的缓冲区保存最近的几个值
* 添加新的观察者的时候会向新增的订阅者重新发送最后几个值
使用的时候需要设置缓存区大小：
```ts
var subject = new Rx.ReplaySubject(2); // 设定缓存区，给新观察者发送最近两个值
```
