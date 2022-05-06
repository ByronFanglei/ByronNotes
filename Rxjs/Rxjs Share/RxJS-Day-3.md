# ReactiveX JavaScript（RxJS）- Day 3

## 快速上手

* 三个步骤：
1. creat：建立一个事件流，称为 stream，在 ReactiveX 中也称为「可观察的物件」Observable
2. Combine：数据流透过操作符 Operators 的组合使用，转换 Observable
3. Listen：监听订阅，在Reactive 中也称为订阅 subscribe，也就是订阅这个事件流，取得资料后续要做的事情

## creat 建立新的 Observable

* 可以直接使用 RxJS 建立 Observable 的 API 比如 fromEvent
* 或者自己重新建立一个 Observable，在ReactiveX 的定义中，我们把它称为 Subject，Subject 有三个方法：
  * next：用来触发新的事件资料，呼叫 `next` 并传入新的事件资料后，在订阅步骤就会吃到有新事件发生了
  * error：只有第一次呼叫会在订阅步骤触发，当整个流程发生错误时，可以呼叫 `error` 并传入作资料，用来告知错误内容，同时整个Observable 就算是结束了
  * complete：只有第一次呼叫会在订阅步骤触发，当整个 Observable 结束时，用来通知已经结束了，由于是单纯的结束了，`complete()`方法不用带入任何参数


## Combine 使用 Operators 组合/转换来源 Observable

* RxJS 中 Operators 一半用来处理数据流，比如之前使用过的 filter 、map 等，Observable 通过 pipe 来连接 Operators 

```typescript
  import { fromEvent } from "rxjs";
  import { filter, map } from 'rxjs/operators';
  
  fromEvent(document, 'click')
    .pipe(
      filter((_, index) => index % 2 === 0), // 使用 filter operator
      map((event: MouseEvent) => ({ x: event.x, y: event.y })) // 使用 map operator
    )
```

* ReactiveX 根据不同类型制定出了不同分类的Operators来处理Observables，大致包含几类：

  * 建立类Creation Operators
  * 组合建立类Join Creation Operators
  * 转换类Transformation Operators
  * 过滤类Filtering Observables
  * 组合类Join Operators
  * 多播类Multicasting Operators
  * 错误处理类Error Handling Operators
  * 工具类Utility Operators
  * 条件/布林类Conditional and Boolean Operators
  * 数学/聚合类Mathematical and Aggregate Operators

## Listen 订阅Observable

* 当有了 Observable 后， 我们就可以直接订阅来获取资料了

```typescript
import { Subject } from "rxjs";
// 创建一个 Observable
const source$ = new Subject();

const observer = {
	next: (data) => console.log(data),
	error: (err) => console.log(err),
	complete: () => console.log("complete"),
};

// 订阅
const subscription = source$.subscribe(observer);
source$.next(1); // 1
source$.next(2); // 2

// 取消订阅
subscription.unsubscribe();

source$.next(3);
console.log(subscription.closed); // true 用来判断是否已取消订阅、发生错误或完成
```