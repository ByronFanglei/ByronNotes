# ReactiveX JavaScript（RxJS）- Day 16

## 「组合/建立类型」的operators

### concat

* 概括：concat可以将数个 Observables 组合成一个新的 Observable，并且在每个 Observable 结束后才接续执行下一个 Observable，想像看一下三个 Observables 是什么情况

```typescript
import { of, concat } from "rxjs"; 

const sourceA$ = of(1, 2);
const sourceB$ = of(3, 4);
const sourceC$ = of(5, 6);

// 这典型的是 callback hell  
sourceA$.subscribe({
	next: (data) => console.log(data),
	complete: () =>
		sourceB$.subscribe({
			next: (data) => console.log(data),
			complete: () =>
				sourceC$.subscribe({
					next: (data) => console.log(data),
				}),
		}),
});

// 这个时候我们使用 concat 可以轻而易举的结局了

concat(of(1, 2), of(3, 4), of(5, 6)).subscribe((data) => {
	console.log(data);
});
```

![callback hell](https://miro.medium.com/max/1400/1*zxx4iQAG4HilOIQqDKpxJw.jpeg)


### merge

* 概括：merge跟 concat 类似，但会同时启动参数内所有的Observable，因此会有「平行处理」的感觉，相当于每秒把当前接受的 Observables 的信息都打出来，如果有就打出来，没有则空

```typescript
import { merge, interval } from "rxjs"; 

const sourceA$ = interval(1000).pipe(map((data) => `一：${data}`));
const sourceB$ = interval(3000).pipe(map((data) => `二：${data}`));
const sourceC$ = interval(5000).pipe(map((data) => `三：${data}`));

const subscription = merge(sourceA$, sourceB$, sourceC$)
	.subscribe((data) => console.log(`merge 实例： ${data}`));

setTimeout(() => {
	subscription.unsubscribe();
}, 10000);

// 弹珠图
// sourceA$: --0--1--2--3--4--5--6--7--8--9|
// sourceB$: --------0--------2--------3---|
// sourceC$: --------------0--------------1|

// merge(sourceA$, sourceB$, sourceC$)
// --0--1--(2,0)--3--(4,,0)--(5,2)--6--7--(8,3)--(9,,1)|
```


### zip

* 概括：把Observables合并在一起，且资料是「一组一组合并在一起的」，实际上在使用时，zip会将传入的Observables 依次组合在一起成为一个数组，已经被组合过的就不会再次被组合，相当与就是把当前接受的 Observables 进行打印，如果有一个打印出来，那就要等待其他的打印出来后形成一个数组抛出来

```typescript
import { zip, interval } from "rxjs"; 

const sourceA$ = interval(1000).pipe(map((data) => `一：${data}`));
const sourceB$ = interval(3000).pipe(map((data) => `二：${data}`));
const sourceC$ = interval(5000).pipe(map((data) => `三：${data}`));

const subscription = zip(sourceA$, sourceB$, sourceC$)
	.subscribe((data) => console.log(data));

setTimeout(() => {
	subscription.unsubscribe();
}, 10000);
```

* 弹珠图

![](https://ithelp.ithome.com.tw/upload/images/20201001/20020617XbbItSUgQp.jpg)



### partition

* 概括：将Observable 依照规则拆成两条Observable，有两个参数：source : 来源Observable；predicate : 用来拆分的条件，是一个function，每次事件发生都会将资料传入此function，并会传是否符合条件(true/false)，符合条件的(true)会被归到一条Observable，不符合条件的则被归到另外一条Observable。


```typescript
import { partition } from "rxjs"; 

const source$ = interval(1000);
const filter = (data) => data % 2 !== 0;
// 哈哈哈 这个有点 react hooks那味了
const [sourceEven$, sourceOdd$] = partition(source$, filter);

const subscriptionA = sourceEven$.subscribe((data) =>
	console.log(`partition 奇数: ${data}`)
);
const subscriptionB = sourceOdd$.subscribe((data) =>
	console.log(`partition 偶数: ${data} `)
);

setTimeout(() => {
	subscriptionA.unsubscribe();
	subscriptionB.unsubscribe();
}, 10000);

```



## 总结
* concat：用来「串接」数个Observables，会依序执行每个Observable，上一个Observable 「完成」后才会执行下一个Observable。

* merge：用来「同时执行」数个Observables，所有Observables 会同时执行，并只在一条新的Observable 上发生事件。

* zip：一样「同时执行」数个Observables，差别是会将每个Observable 的资料「组合」成一个新的事件值，在新的Observable 上发生新事件。

* partition：依照指定逻辑，将一条Observable 拆成两条Observables。

