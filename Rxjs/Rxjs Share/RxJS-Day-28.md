# ReactiveX JavaScript（RxJS）- Day 28

## RxJS 工具类型Operators

* tap / toArray / delay / delayWhen

```typescript
import { from, fromEvent, iif, interval, of, range, throwError } from "rxjs";
import {
	delay,
	delayWhen,
	filter,
	map,
	switchMap,
	take,
	tap,
	toArray,
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

### tap

* 概念：主要就是用来处理side effect（副作用） 的，在使用各种operators 时，我们应该尽量让程式内不要发生side effect，但真的有需要处理side effect 时，可以使用 tap 把「side effect」和「非side effect」隔离，未来会更加容易找到问题发生的地方

```typescript
interval(1000)
	.pipe(
		map((data) => data * 2),
		tap((data) => console.log("目前资料", data)),
		map((data) => data + 1),
		tap((data) => console.log("目前资料", data)),
		take(10)
	)
	.subscribe((data) => {
		console.log(`tap: ${data}`);
	});

```


### toArray

* 概念：来源Observable 发生事件时，不会立即发生在新的Observable 上，而是将资料暂存起来，当来源Observable结束时，将这些资料组合成一个数组发生在新的Observable 上

```typescript
interval(500).pipe(take(3), toArray()).subscribe(log);

```

toArray还有一种妙用，就是拿来处理阵列相关的逻辑，我们可以使用of、from或 range 等建立Observable 的operator 来产生一个固定的Observable，透过Observable 及 pipe 是一笔一笔资料流入所有operators 的特性，来处理资料

```typescript
// rxjs 实现
from([1, 2, 3, 4, 5, 6, 7, 8, 9])
	.pipe(
		map((data) => data * data),
		filter((data) => data % 3 == 0),
		toArray()
	)
	.subscribe(log);

// js 数组方式实现
console.log(
	[1, 2, 3, 4, 5, 6, 7, 8, 9]
		.map((data) => data * data)
		.filter((data) => data % 3 == 0)
);

// 看着好像没什么区别，但实际上效能会好上很多，因为Observable 不会把整个阵列全部带入 map 再带入 filter 内；同时还可以享有更多operators 的支援！
```


### delay

* 概念：delay会让来源Observable 延迟一个指定时间(毫秒)再开始。

```typescript
of(1, 2, 3).pipe(delay(1000)).subscribe(log);

```



### delayWhen

* 概念：自行决定来源Observable 每次事件延迟发生的时机点，在 delayWhen 内需要传入一个delayDurationSelector callback function，delayWhen会将事件资讯传入，而 delayDurationSelector 需要回传一个Observable，当此Observable 发生新事件时，才会将来源事件值发生在新的Observable 上，delayWhen 第二个参数(非必须)是一个 subscriptionDelay Observable，delayWhen可以透过这个Observable 来决定来源Observable 开始的时机点；当整个Observable 订阅开始时，delayWhen会订阅这个subscriptionDelayObservable ，当事件发生时，才真正订阅来源Observable，然后退订阅subscriptionDelayObservable

```typescript
const del = (data) => of(data).pipe(delay((data % 2) * 2000));
interval(1000)
	.pipe(
		delayWhen(
			(value) => del(value),
			// 当点击页面后开始触发 Observable 开始，如果没有这个参数，那么会直接开始
			fromEvent(document, "click")
		),
		take(10)
	)
	.subscribe(log);
	
```


## 总结

* tap：可以用来隔离「side effect」以及「非side effect」，在Observable 运作过程中，不论是next()、error()或complete()，只要有side effect 逻辑都建议放到 tap 内处理。

* toArray：将来源Observable 资料汇整成一个阵列。toArray可以应用来处理阵列资料。

* delay：延迟一段时间后，才开始运行来源Observable。

* delayWhen：可自行设计Observable，来决定来源Observable 每个事件的延迟逻辑。




