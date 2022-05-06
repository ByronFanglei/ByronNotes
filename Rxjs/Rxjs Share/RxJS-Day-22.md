# ReactiveX JavaScript（RxJS）- Day 22

## RxJS 过滤类型Operators

* take / takeLast / takeUntil / takeWhile


```typescript
// 公共代码
import { fromEvent, interval, range } from "rxjs";
import { map, take, takeLast, takeUntil, takeWhile } from "rxjs/operators";

const log = {
	next: (data) => {
		console.log(data);
	},
	error: (err) => {
		console.log(err);
	},
	complete: () => {
		console.log("complete");
	},
};

```

### take

* 概念：take是从事件开始时抓取前面N 次的事件值

```typescript
interval(1000).pipe(take(5)).subscribe(log);

```


### takeLast

* 概念：takeLast 则是从抓取Observable 最后N 次的事件值，因次 takeLast 会等到Observable 结束后，才会得到最后几次事件的资料。

```typescript
range(1, 5).pipe(takeLast(3)).subscribe(log);

```

### takeUntil

* 概念：takeUntil会持续触发来源 Observable 的事件值，直到(until) 指定的另外一个Observable 发生新事件时，才会结束

```typescript
const click$ = fromEvent(document, "click");
const source$ = interval(1000).pipe(map((data) => data + 1));

source$.pipe(takeUntil(click$)).subscribe(log);

```


### takeWhile

* 概念：takeWhile内需要传入一个callback function，这个callback function 决定 takeWhile 发生事件的时机，只要事件值持续符合callback function 内的条件，就会持续产生事件，直到不符合条件后结束 以及 以及 一个 inclusive 参数 **代表是否要包含判断不符合条件的那个值**，预设为false，当设为 true 时，发生结束条件的那次事件值也会被包含在要发生的事件内

```typescript
const source$ = interval(1000);
source$.pipe(takeWhile((data, index) => data <= 7, true)).subscribe(log);

```

## 总结

* take：代表要让前N 次事件可以发生，符合数量后结束目前的Observable。

* takeLast：代表要让后N 次事件可以发生，因此需要来源Observable 结束。

* takeUntil：会持续让来源Observable 事件发生，直到指定的另一个Observable 发生新事件了，结束目前的Observable。

* takeWhile：可以判断资料是否符合条件，只要资料符合条件，事件就会持续发生，当资料不符合条件，目前的Observable 就会结束。