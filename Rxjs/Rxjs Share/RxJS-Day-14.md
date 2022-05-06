# ReactiveX JavaScript（RxJS）- Day 14

## RxJS 建立类型Operators 

* EMPTY / of / range / iif / throwError / ajax

### EMPTY

* 概念：EMPTY就是一个Observable，没有任何事件就直接结束了

```typescript
import { EMPTY } from "rxjs";

EMPTY.subscribe({
	next: (data) => console.log(`空的观察者： ${data}`),
	complete: () => console.log("空的观察者：END"),
});

```


### of

* 概念：就是将传进去的值当作一条Observable，当值都发送完后结束，直接上程式

```typescript
import { of } from "rxjs";

of(1, 2, 3, 4, 5).subscribe({
	next: (data) => console.log(`数据：${data}`),
	complete: () => console.log("END"),
});

```


### range

* 概念：依照一个范围内的数列资料建立Observable，两个参数
> start : 从哪个数值开始
> count : 建立多少个数值的数列

可以理解为打印出对应范围内的值

```typescript
import { range } from "rxjs";

range(5, 10).subscribe({
	next: (data) => console.log(`数据：${data}`),
	complete: () => console.log("END"),
});

```


### iif

* 概念：会透过条件来决定产生怎么样的Observable，有三个参数
> 1. condition : 传入一个function，这个function 会回传布林值。
> 2. trueResult : 当呼叫condition 参数的function 回传 true 时，使用trueResult 的Observable
> 3. falseResult : 当呼叫condition 参数的function 回传 false 时，使用falseResult 的Observable

可以理解为if语句，当参数一成立执行参数二，否则执行参数三

```typescript
import { iif } from "rxjs";

const emitOneIfEven = (data) => {
	return iif(() => data % 2 === 0, of("Byron", "Hello"), EMPTY);
};

emitOneIfEven(1).subscribe({
	next: (data) => console.log(`例子1: ${data}`),
	complete: () => console.log("EMPTY"),
});
emitOneIfEven(2).subscribe({
	next: (data) => console.log(`例子2: ${data}`),
	complete: () => console.log("EMPTY"),
});

```



### throwError

* 概念：用来让整条Observable 发生错误( error()) 用的！因此订阅时要记得使用 error 来处理，同时当错误发生时，就不会有「完成」发生

```typescript
import { throwError } from "rxjs";

const source$ = throwError("抛出错误！！！");

source$.subscribe({
	next: (data) => console.log(`next: ${data}`),
	complete: () => console.log(`END`),
	error: (err) => console.log(`ERROR: ${err}`),
});
```


### ajax

* 概念：这个应该很熟悉把，算是比较特殊的工具operator，放在 rjxs/ajax 下，而功能看名字就知道，是用来发送HTTP 请求抓API 资料的，会回传ajaxResponse 格式

```typescript
import { ajax } from "rxjs/ajax";
const source$ = ajax({
	url: "https://api.github.com/repos/reactivex/rxjs/issues",
	// method: 'GET'
});
source$.subscribe((data) => console.log(data));

```

* 虽然有很多现成的工具可以去抓API 资料，例如原生的fetch API，或是axios、jQuery等套件，但使用 ajax 的好处是已经将资料包成Observable 了，我们可以轻易地直接跟许多现有的Operators 组合出各式各样的玩法，若使用其它套件，则需要自行包装成Observable。

* ajax建立的也是cold observable，也就是说在真正订阅发生前，并不会真的去呼叫HTTP 请求，也就是我们在介绍functional programming 时提到的「延迟评估」的特色，在真正需要(也就是订阅) 时才真的去发送HTTP


## 总结

* EMPTY：用来产生一条「空的Observble」，也就是没有发生任合事件值，就结束。

* of：用里面的参数当作每次事件的资料。

* range：用一定范围内的数值资料作为事件的资料。

* iif：依照第一个参数的条件，决定要使用不同的Observable 资料流。

* throwError：让Observable 发生错误。

* ajax：呼叫一个HTTP 请求作为Observable 的事件资料。



