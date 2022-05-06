# ReactiveX JavaScript（RxJS）- Day 23


## RxJS 过滤类型Operators
* skip / skipLast / skipUntil / skipWhile

```typescript
// 公共代码
import { fromEvent, interval, range } from "rxjs";
import {
	skip,
	skipLast,
	skipUntil,
	skipWhile,
	take,
	takeLast,
} from "rxjs/operators";

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

### skip

* 概念：可以传入一个数字，当订阅开始时，会「忽略」前N 个事件值，到第N + 1 的事件值才会收到资料

```typescript
interval(1000).pipe(take(5)).subscribe(log);

```


### skipLast

* 概念：会忽略整个Observable 的最后N 次事件值, 跟昨天提到的 takeLast 不同的地方是，skipLast不用等到整个Observable 结束才知道要怎么开始抓资料的值，从skipLast 的实作 来看的话，会在前面N 次事件发生时不做任何事情，当N + 1 次事件发生时，才把资料流从头开始依照每次新事件发生时把资料送出。

```typescript
interval(1000).pipe(take(10), skipLast(3)).subscribe(log);
// interval(1000).pipe(take(10), takeLast(3)).subscribe(log);

```

### skipUntil

* 概念：会持续忽略资料，直到指定的Observable 发出新的事件时，才开始资料

```typescript
const click$ = fromEvent(document, "click");
const source$ = interval(1000);

source$.pipe(skipUntil(click$)).subscribe(log);

```


### skipWhile

* 概念：需要传入一个callback function，在这个function 会决定忽略目前的事件资料的条件，只要符合这个条件，会持续忽略事件值，直到条件不符合为止

```typescript
interval(1000)
	.pipe(skipWhile((data) => data < 5))
	.subscribe(log);

```


## 总结

* skip：从订阅开始后忽略指定数量的事件资料。

* skipLast：依照指定的数量，忽略整个Observable 最后的事件数量。

* skipUntil：持续忽略目前Observable 的事件资料，直到另一个Observable 发生事件为止。

* skipWhile：持续忽略目前Observable 的事件资料，直到事件资料值不符合指定条件为止。