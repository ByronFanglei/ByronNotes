# ReactiveX JavaScript（RxJS）- Day 17

## 「组合/建立类型」的operators

### combineLatest

* 概括：参数为 Observable 组合的数组，combineLatest跟昨天介绍过的 zip 非常像，差别在于 zip 会依序组合，而 combineLatest 会在资料流有事件发生时，直接跟目前其他资料流的「最后一个事件」组合在一起，
* 相当于是：当第一个抛出值后等其他抛出值后合并在一起抛出来，也就是说当所有的 Observable 都有值的话 才会抛出来，第一次抛出来的时间是当前 Observable 中需要时间最久的那个来定的

```typescript
import { interval, combineLatest } from 'rxjs'
import { map } from 'rxjs/operators'

const sourceA$ = interval(1000).pipe(map((data) => `一：${data + 1}`));
const sourceB$ = interval(3000).pipe(map((data) => `二：${data + 1}`));
const sourceC$ = interval(5000).pipe(map((data) => `三：${data + 1}`));

const subscription = combineLatest([sourceA$, sourceB$, sourceC$]).subscribe(
	(data) => {
		console.log(data);
	}
);

setTimeout(() => {
	subscription.unsubscribe();
}, 10000);

```



### forkJoin

* 概括：参数为 Observable 组合的数组，直到每个Observable 都「结束」后，将每个Observable 的「最后一笔值」组合起来
* 相当于是：数组内的 Observable 都结束后 将每个 Observable 的最后一个值组成 数组 抛出来

```typescript
import { interval, forkJoin } from 'rxjs'
import { map, tap, take } from 'rxjs/operators'

const sourceA$ = interval(1000).pipe(
	map((data) => `一：${data + 1}`),
	tap(() => console.log("111")),
	take(5) // 会在事件发生指定次数后 结束
);
const sourceB$ = interval(3000).pipe(
	map((data) => `二：${data + 1}`),
	take(4)
);
const sourceC$ = interval(5000).pipe(
	map((data) => `三：${data + 1}`),
	take(3)
);

forkJoin([sourceA$, sourceB$, sourceC$]).subscribe({
	next: (data) => console.log(data),
	complete: () => console.log("END"),
});

```

* forkJoin的一些使用场景：平行发送多个没有顺序性的 网络请求，因为 网络请求只会发生一次回传就结束，如果每个请求之前没有顺序性，那么一起发送会是比较快可以拿到全部资料的方法，有点想 Promise.all



### race

* 概括：参数为 Observable 组合的数组，同时订阅所有的 Observable 当有一个 Observable 发生第一次事件后，就会取消订阅其他的 Observable，有点像 Promise.race

```typescript
import { interval, race } from 'rxjs'
import { map } from 'rxjs/operators'

const sourceA$ = interval(1000).pipe(map((data) => `一：${data + 1}`));
const sourceB$ = interval(3000).pipe(map((data) => `二：${data + 1}`));
const sourceC$ = interval(5000).pipe(map((data) => `三：${data + 1}`));

const subscription = race([sourceA$, sourceB$, sourceC$]).subscribe((data) => {
	console.log(data);
});

setTimeout(() => {
	subscription.unsubscribe();
}, 10000);

```


## 总结

* combineLatest：同时订阅所有内部Observables，并将内部Observables 里面的最后一次事件资料组合起来。
* forkJoin：同时订阅所有内部Observables，并将内部Observables 「完成」前的最后一个事件资料组合起来。
* race：同时订阅所有内部Observables，当其中一个Observable 先发生第一次事件后，以此Observable 为主，并将其他Observable 取消订阅。