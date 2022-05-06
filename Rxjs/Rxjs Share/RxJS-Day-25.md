# ReactiveX JavaScript（RxJS）- Day 25

## RxJS 过滤类型Operators 

```typescript
// 公共代码
import { from, interval, Subject, timer } from "rxjs";
import {
	audit,
	auditTime,
	debounce,
	debounceTime,
	sample,
	sampleTime,
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

* sampleTime / sample / auditTime / audit / debounceTime / debounce

### sampleTime

* 概念：有「定期取样」的意思，可以指定一个周期时间，当Observable 被订阅时，就会依据指定的周期时间，每经过这段时间就从来源Observable 内取得这段时间最近一次的事件资料

```typescript
const subject$ = new Subject();
subject$.pipe(sampleTime(1000), take(2)).subscribe(log);

setTimeout(() => subject$.next(0), 0);
setTimeout(() => subject$.next(999), 999);
setTimeout(() => subject$.next(1000), 1000);
setTimeout(() => subject$.next(1500), 1500);
setTimeout(() => subject$.next(2001), 2001);

```


### sample

* 概念：是单纯「取样」的意思，我们可以传入一个 notifer 的Observable，每 notifier 有新事件发生时，sample就会在来源Observable 上取一笔最近发生过的事件值，因此透过 sample 我们可以自行决定取样的时机点。

适用于我们需要自行决定取样点逻辑的时候

```typescript
const notifier$ = new Subject();
const source$ = interval(1000);
source$.pipe(sample(notifier$), take(2)).subscribe(log);

setTimeout(() => notifier$.next(999), 999);
setTimeout(() => notifier$.next(1000), 1000);
setTimeout(() => notifier$.next(1500), 1500);
setTimeout(() => notifier$.next(2100), 2100);

```


### auditTime

* 概念：依照「新事件发生后的指定时间内」来处理

```typescript
interval(1500).pipe(auditTime(1500), take(2)).subscribe(log);

```

因为 auditTime 会在来源Observable 有新事件发生时才会开始计算时间，因此至少一定会有一次事件当作新Observable 的事件资料，而 sampleTime 因为是「时间循环」的关系，可能在某个时段内来源Observable 都没有新的事件，因此在新的Observable 上也不会有新的事件资料


### audit

* 概念：audit和 auditTime 非常类似，都是在一个指定的时间发生时让来源Observable 最近一次的事件发生在新的Observable 上，差别在 auditTime 是直接指定时间，而 audit 则是传入一个durationSelectorcallback function，audit 会将来源Observable 事件值传入callback function，同时回传一个Observable 或Promise，audit会依此资讯来决定下次事件发生的时机，处理逻辑如下：

1. 每当来源Observable 发生新的事件时，就会订阅 durationSelector 回传的资料流
2. 在durationSelector 回传的资料流有新的事件前，来源Observable 的事件都不会发生在新的Observable 上
3. 直到从 durationSelector 回传的资料流发生第一次事件后，再将来源Observable 这段时间内发生事件的「最后一笔事件值」发生在新的Observable 上，同时退订 duratorSelector 的资料流
4. 之后等待来源资料流下一次事件发生，并重复步骤1.

```typescript
const durationSelector = (value) => interval(value * 1200);
interval(1000).pipe(audit(durationSelector)).subscribe(log);

```


### debounceTime

* 概念：可以指定一个时间间隔，当来源Observable 有新事件资料发生时，会等待这段时间，如果这段时间内没有新的事件发生，将这个资料值发生在新的Observable 上；如果在这段等待时间有新的事件发生，则原来事件不会发生在新的资料流上，并持续等待，

可以理解为 在指定的一段时间内没有数据发出，那么返回最后的数据，如果有那么重新等待指定的时间

```typescript
const subject$ = new Subject();
subject$.pipe(debounceTime(1000)).subscribe(log);
setTimeout(() => subject$.next(0), 0);
setTimeout(() => subject$.next(1100), 1100);
setTimeout(() => subject$.next(1999), 1999);
setTimeout(() => subject$.next(2200), 2200);
setTimeout(() => subject$.next(3300), 3300);
setTimeout(() => subject$.complete(), 3500);

```


### debounce

* 概念：当来源Observable 有新事件发生时，依照另外一个Observable 来决定要再多长时间内没有新的事件发生，才允许此事件发生在新的Observable 上

```typescript
const durationSelector = (value) => interval(value * 1000);

interval(3000)
	.pipe(debounce(durationSelector))
	.subscribe((data) => {
		console.log(`debounce 示範: ${data}`);
	});
	
```


## 总结
* sampleTime：每个一个循环时间取一次时间区段内来源Observable 最新的资料

* sample：依照指定的Observable 事件发生时机来取时间区段内来源Observable 最新的资料

* auditTime：当来源Observable 有新事件发生时，依照指定时间取得事件发生后这段时间内来源Observable 最新的资料

* audit：当来源Observable 有新事件发生时，依照另外一个Observable 来决定要在多长的时间内取得来源Observable 最新的资料

* debounceTime：当来源Observable 有新事件发生时，须在指定时间内没有新的事件发生，才允许此事件发生在新的Observable 上

* debounce：当来源Observable 有新事件发生时，依照另外一个Observable 来决定要再多长时间内没有新的事件发生，才允许此事件发生在新的Observable 上








