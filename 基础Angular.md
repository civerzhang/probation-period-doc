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

## 附：HTML attribute 与 DOM property 的对比

* attribute 是由 HTML 定义的。property 是由 DOM (Document Object Model) 定义的。
* 少量 HTML attribute 和 property 之间有着 1:1 的映射，如 id。
* 有些 HTML attribute 没有对应的 property，如 colspan。
* 有些 DOM property 没有对应的 attribute，如 textContent。
* 大量 HTML attribute 看起来映射到了 property…… 但却不像你想的那样！

attribute 初始化 DOM property，然后它们的任务就完成了。property 的值可以改变；attribute 的值不能改变。

HTML 的 value 这个 attribute 指定了初始值；DOM 的 value 这个 property 是当前值。

`模板绑定是通过 property 和事件来工作的，而不是 attribute。`

**在 Angular 的世界中，attribute 唯一的作用是用来初始化元素和指令的状态。 当进行数据绑定时，只是在与元素和指令的 property 和事件打交道，而 attribute 就完全靠边站了。**

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
