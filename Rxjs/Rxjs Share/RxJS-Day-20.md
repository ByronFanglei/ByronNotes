# ReactiveX JavaScript（RxJS）- Day 20

## RxJS 组合类型 Operators

* switchAll / concatAll / mergeAll / combineAll / startWith


### switchAll
* 概念：switchMap可以将来源Observable 事件的「资料」转换成Observable，switchAll则非常相似，是将来源事件的「Observable」转换成另一个Observble


```typescript
import { Subject, timer } from "rxjs";
import { map, switchAll, take } from "rxjs/operators";

const subject$ = new Subject();

const generateStream = (round) =>
	timer(0, 1000).pipe(
		map((input) => `数据源：data: ${round} input ${input}`),
		take(3)
	);

const source$ = subject$.pipe(map((data) => generateStream(data)));

source$.pipe(switchAll()).subscribe((data) => console.log(data));

// 第一次事件
subject$.next(1);

// 第二次事件
setTimeout(() => {
	subject$.next(2);
}, 4000);

// 第三次事件
setTimeout(() => {
	subject$.next(3);
}, 5000);
```

* switchAll 与 switchMap 不同之处：
  * switchMap：会将 callback function 呼叫回传的Observable 进行订阅，因此Observable 的来源是从callback function 转换过来的
  * switchAll：没有这个callback function，它的来源是从前一个资料流过来的，因此前一个资料流的「资料」必须是个Observable

* switchAll 与 switchMap 相同处：当收到新的Observable 要订阅时，都会退订上一个Observable，因此可以确保永远都只有最后一个Observable 正在执行


### concatAll

* 概念：会等待前一个Observable 完成，在开始继续新的Observable 资料流订阅，因此可以确保每个资料流都执行至完成

```typescript
import { Subject, timer } from "rxjs";
import { map, concatAll, take } from "rxjs/operators";

const subject$ = new Subject();

const generateStream = (round) =>
	timer(0, 1000).pipe(
		map((input) => `数据源：data: ${round} input ${input}`),
		take(3)
	);

const source$ = subject$.pipe(map((data) => generateStream(data)));

source$.pipe(concatAll()).subscribe((data) => console.log(data));

// 第一次事件
subject$.next(1);

// 第二次事件
setTimeout(() => {
	subject$.next(2);
}, 4000);

// 第三次事件
setTimeout(() => {
	subject$.next(3);
}, 5000);
```


### mergeAll

* 概念：得到新的资料流后会直接订阅，且不退订之前的资料流，因此所有资料流会依照各自发生的时间直接的发生在 mergeAll 建立的资料流上，且由于 mergeAll 会同时订阅多个Observable，当要订阅的Observable 数量庞大时容易产生效能的问题，因此 mergeAll 还可以带入一个 concurrent 参数，代表同时最多可以订阅几个Observable，预设值为正无限大。当参数为 1 的时候就相当于调用了 concatAll 方法

```typescript
import { Subject, timer } from "rxjs";
import { map, mergeAll, take } from "rxjs/operators";

const subject$ = new Subject();

const generateStream = (round) =>
	timer(0, 1000).pipe(
		map((input) => `数据源：data: ${round} input ${input}`),
		take(3)
	);

const source$ = subject$.pipe(map((data) => generateStream(data)));

source$.pipe(mergeAll(2)).subscribe((data) => console.log(data));

// 第一次事件
subject$.next(1);

// 第二次事件
setTimeout(() => {
	subject$.next(2);
}, 4000);

// 第三次事件
setTimeout(() => {
	subject$.next(3);
}, 5000);
```


### combineAll

* 概念：把资料流的资料组合在一起，规则是每当有资料流发生新事件值时，将这个事件值和其他资料流最后一次的事件值组合起来

```typescript
import { Subject, timer } from "rxjs";
import { map, combineAll, take } from "rxjs/operators";

const subject$ = new Subject();

const generateStream = (round) =>
	timer(0, 1000).pipe(
		map((input) => `数据源：data: ${round} input ${input}`),
		take(3)
	);

const source$ = subject$.pipe(map((data) => generateStream(data)));

source$.pipe(combineAll()).subscribe((data) => console.log(data));

subject$.next(1);

setTimeout(() => {
	subject$.next(2);
	// 结束事件流，不然 combineAll 会持续等待到结束
	subject$.complete();
}, 3000);

```

### startWith

* 概念：在一个Observable 内加上一个起始值，也就是订阅产生时会立刻最先收到的一个值，例如在前两天练习 pairwise 时，会因为第一个事件值没有「上一个事件值」被忽略，因此改用 scan 来解决，但也可以使用startWith，会更加简单


```typescript
import { interval, Subject, timer } from "rxjs";
import { map, pairwise, take, startWith, scan } from "rxjs/operators";

interval(1000)
	.pipe(
		take(6),
		map((data) => data + 1),
		startWith(0),
		// scan((accu, value) => [accu === null ? 0 : accu[1], value], null) // 使用 scan 生成首项
		pairwise()
	)
	.subscribe((data) => console.log(data));

```


## 总结

* xxxAll系列和 xxxMap 系列处理「上一个」资料流的方式一样，差别在于 xxxAll 是使用别人传给我们的Observable of Observable，而 xxxMap 必须自行撰写转换成Observable 的规则。

* cominbeAll和 combineLatest 行为也非常类似，combineAll的资料来源是Observable of Observable，而 combineLatest 则必须明确指定要组合哪写Observables。

* startWith适合用在一些希望订阅Observable 时就能够有第一笔预设值的情境。
