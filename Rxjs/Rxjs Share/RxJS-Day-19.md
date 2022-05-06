# ReactiveX JavaScript（RxJS）- Day 19

## 今天介绍几个使用频率非常高、功能非常类似，又有很大差别的转换类型operators：
* switchMap/ concatMap/ mergeMap/ exhaustMap。

### switchMap

* 先来看一个问题，假如一个功能是获取网络数据，一个是点击后重新获取网络数据，我们可以设计两个 observable

```typescript
import { ajax } from "rxjs/ajax";
import { fromEvent } from "rxjs";
import { map, switchMap } from "rxjs/operators";

interface result {
	[data: string]: any;
}

const click$ = fromEvent(document, "click");
const result$ = ajax("https://api.github.com/repos/reactivex/rxjs/issues").pipe(
	map((result: result) => result?.response)
);

// 当实现这个功能的时候，引发了 callback hell 这个前面也有提到过要尽量避免
click$.subscribe(() => {
	result$.subscribe((data) => {
		console.log(data);
	});
});

// 接下来我们我们使用 switchMap 来试试，
// 概念：switchMap内是一个 project function 传入的参数为前一个 Observable 的事件值，同时必须回传一个 Observable；因此可以帮助我们把来源事件值换成另外一个Observable，而 switchMap 收到这个Observable 后会帮我们进行订阅的动作，再把订阅结果当作新的事件值。

click$.pipe(switchMap((data) => result$)).subscribe((data) => {
	console.log(data);
});
```

* switchMap 还有另外一个重点，就是「切换」(switch)的概念，当来源Observable 有新的事件时，如果上一次转换的 Observable 还没完成，会退订上一次的资料流，并改用新的Observable 资料流

```typescript
import { interval } from "rxjs";
import { switchMap } from "rxjs/operators";

interval(3000)
	.pipe(
		switchMap((data) => {
			console.log(`interval: ${data}`);
			return timer(0, 1000);
		})
	)
	.subscribe((data) => console.log(data));
	
// 来源Observable (interval(3000)) 每次有新事件发生时，会产生新的Observable (timer(0, 1000))，如果上一次Observable 没有完成，会被退订阅掉，「切换」成新的Observable。因此每次都只会产生 0, 1, 2 的循环
```

* switchMap 使用场景：当我们需要关注在「新事件产生的新资料流，过去的资料流不再重要时」，像是 网络请求 这种通常需要最新结果的情境，非常适合使用switchMap。比如说 连续切换日期，当我们切换到 2 月查看数据，但是 2 月数据还没有回来我们就直接查看 3 月的，这中情况就可以使用 switchMap



### concatMap
* 概念：concatMap一样在每次事件发生时都会产生新的Observable，不过 concatMap 会等前面的Observable 结束后，才会「接续」(concat)新产生的Observable 资料流

```typescript
import { interval } from "rxjs";
import { concatMap, take } from "rxjs/operators";

interval(3000)
	.pipe(
		concatMap((data) => {
			console.log(`interval: ${data}`);
			return timer(0, 1000).pipe(take(5));
		})
	)
	.subscribe((data) => console.log(data));

// 使用 concatMap 时，转换后的Observable 基本上都必须设定结束条件，也就是要确保会完成(complete)，否则很容易就会产生不可预期的问题(就是一直不会结束...)，当每个资料流都非常重要不可取消，且必须照着顺序执行时，使用 concatMap 就对了！
```

### mergeMap

* 概念：mergeMap会把所有被转换成的Observable 「合并」(merge)到同一条资料流内，因此会有平行处理的概念，也就是每此转换的Observable 都会直接订阅，不会退订上一次的Observable，也不会等待上一次的Observable 结束，因此任何目前存在中的Observable 资料流有新事件，都会被转换成整体资料流的事件

```typescript
import { interval, timer } from "rxjs";
import { mergeMap, map } from "rxjs/operators";

interval(300).pipe(
	mergeMap((input) =>
		timer(0, 3000).pipe(
			map((data) => {
				return `timer: ${data}, interval: ${input}`;
			})
		)
	)
)
.subscribe((data) => console.log(data));
```

### exhaustMap

* 概念：来源事件发生时，如果上一次转换的 Observable 尚未结束，就不会产生新的Observable

```typescript
import { interval } from "rxjs";
import { exhaustMap, take } from "rxjs/operators";

const firstInterval = interval(1000).pipe(take(10));
const secondInterval = interval(1000).pipe(take(2));

firstInterval
	.pipe(
		exhaustMap((f) => {
			console.log(`first interval: ${f}`);
			return secondInterval;
		})
	)
	.subscribe((data) => console.log(data));
```


## 总结

* switchMap：「切换」的概念，退订阅上次未完成的资料流，订阅新的资料流；若有新事件时过去的资料就不重要了，可以使用此operator。

* concatMap：持续等到上次资料流完成，才继续订阅新的资料流；若执行顺序非常重要，可以使用此opereator；不过要注意每次转换的Observable 都需要有完成，否则永远不会进入下一个Observable。

* mergeMap：上次资料流若未完成，不会退订阅，且继续订阅新的资料流；若资料流顺序相对不重要，可以使用此operator，整体效率会比较快。

* exhaustMap：若上次资料流未完成，则忽略订阅这次的资料流；若希望避免产生太多资料流，可以考虑使用此operator。
