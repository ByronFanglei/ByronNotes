# ReactiveX JavaScript（RxJS）- Day 26

## RxJS 条件/布林类型Operators

* isEmpty / defaultIfEmpty / find / finxIndex / every

```typescript
import { EMPTY, interval, Subject } from "rxjs";
import {
	defaultIfEmpty,
	every,
	find,
	findIndex,
	isEmpty,
	map,
	take,
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

### isEmpty

* 概念：isEmpty会判断来源Observable 是否没有「发生过任何事件值」，如果到结束时完全没有任何事件发生过，则会发生 true 事件在新的Observable 上，反之则新的Observable 会发生 false 事件

```typescript
// 测试一
EMPTY.pipe(isEmpty()).subscribe(log);

// 测试二
const source$ = new Subject();
source$.pipe(isEmpty()).subscribe(log);
source$.complete();

// 测试三
interval(500).pipe(take(3), isEmpty()).subscribe(log);

```


### defaultIfEmpty

* 概念：会在Observable 没有任何事件发生就结束时，给予一个预设值

```typescript
// 测试一
const source$ = new Subject();
source$.pipe(defaultIfEmpty("null")).subscribe(log);
setTimeout(() => source$.complete(), 1500);

// 测试二
interval(500).pipe(take(3), defaultIfEmpty("null")).subscribe(log);

```


### find

* 概念：将事件资讯传入此function，并回传是否符合指定的条件，如果符合，就会将目前的事件资料发生在新的Observable 上，同时完成Observable，类似与数组的 find 方法

```typescript
interval(500)
	.pipe(find((data) => data % 3 === 0))
	.subscribe(log);

```


### findIndex

* 概念：当条件符合时，新的Observable 事件资料是「符合条件事件的索引值」，也就是这个事件是来源Observable 的第几次事件，类似与数组的 findIndex 方法

```typescript
interval(500)
	.pipe(findIndex((i) => i === 10))
	.subscribe(log);

```


### every

* 概念：将事件资讯传入此function，并判断来源Observable 是否「全部符合指定条件」，如果符合，在来源Observable 结束时会得到 true 事件；如果不符合，则会在事件资料不符合指定条件同时得到 false 事件并结束

```typescript
interval(500)
	.pipe(
		map((data) => data * 2),
		take(3),
		every((data) => data % 2 === 0)
	)
	.subscribe(log);

```


## 总结

* isEmpty：用来判断来源Observable 是否是空的，也就是没有任何事件发生就结束了。

* defaultIfEmpty：当来源Observable 是空的时候，给予一个预设值。

* find：用来判断来源Observable 是否有符合条件的事件资料，如果有，将此事件资料发生在新的Observable 上，并结束。

* findIndex：用来判断来源Observable 是否有符合条件的事件资料，如果有，将此事件资料在来源

* Observable 的索引值发生在新的Observable 上，并结束。

* every：用来判断来源Observable 的事件是否「全部符合指定条件」。
















