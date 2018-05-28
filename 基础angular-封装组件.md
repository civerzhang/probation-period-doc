# 父子组件的双向绑定（子组件的双向绑定属性通过父组件外部传入）
```html
<!-- 子组件 -->
<p-dropdown [options]="_options" [(ngModel)]="dropDownValue"></p-dropdown>

<!-- 父组件 -->
<waf-dropdown [options]="testoptions" [(ngModel)]="selectedoption"></waf-dropdown>
```
**方案：在父组件wafDropDown中，需实现ControlValueAccessor接口，监听视图的变化，并将视图的变化通知至外部使用该组件处，同时在外部组件传入值时，能绑定至视图中。**
关键实现如下
```ts
// 引入依赖库
import { forwardRef } from '@angular/core';
import { ControlValueAccessor, NG_VALUE_ACCESSOR } from '@angular/forms'

@Component({
  // ...
  providers: [{
    provide: NG_VALUE_ACCESSOR,
    useExisting: forwardRef(() => WafDropdownComponent),
    multi: true
  }]
})
export class WafDropdownComponent implements ControlValueAccessor{

  onValueChange: Function = () => { };
  onTouched: Function = () => { };
  
  _options;
  _ngModel;
  @Input()
  set options(value) {
    console.log("options"+value);
    this._options = value;
  }

//此变量用于发送至使用本组件的父组件中。
  _dropDownValue;

//此方法在本html中值双向绑定至本ts时由angular自动调用
  set dropDownValue(v: any) {
    if (v != this._dropDownValue) {
      this._dropDownValue = v;
      this.onValueChange(this._dropDownValue)
    }
  }
  
  get dropDownValue() {
    console.log(this._dropDownValue);
    return this._dropDownValue;
  }

   /**
   * model=>view
   */
  //此方法用于父组件设置value到本组件中时angular自动调用
  writeValue(value: any) {
    if (value) {
      this._dropDownValue = value;
    }
  }

  /**
   * view=>model
   */
  //此方法用于将本组件的html中的值发生变化时，调用fn方法将值传至父组件中
  registerOnChange(fn) {
    this.onValueChange = fn;
  }
  registerOnTouched(fn: any): void {
    this.onTouched = fn;
  }
```
