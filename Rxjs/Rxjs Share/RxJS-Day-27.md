# ReactiveX JavaScript（RxJS）- Day 27

## RxJS 数学/聚合类型Operators

* min / max / count / reduce

```typescript
import { from, interval, of } from "rxjs";
import { count, max, min, reduce, scan, take } from "rxjs/operators";

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

### min

* 概念：判断来源Observable 资料的最小值，在来源Observable 结束后，将最小值事件资料发生在新的Observable 上，min 内部也可以传入一个 comparer callback function，可以用回调值判断资料的大小

```typescript
// 测试一
of(-1, 1, 2, 3, 4, 5).pipe(min()).subscribe(log);
// 测试二
from([1, 2, 3, 4, 5]).pipe(min()).subscribe(log);

// 测试三
const obj = [
	{ name: "Student A", score: 80 },
	{ name: "Student B", score: 90 },
	{ name: "Student C", score: 60 },
	{ name: "Student D", score: 70 },
];
from(obj)
	.pipe(min((x, y) => x.score - y.score))
	.subscribe(log);

```


### max

* 概念：与 min 相反

```typescript
// 测试一
of(-1, 1, 2, 3, 4, 5).pipe(max()).subscribe(log);
// 测试二
from([1, 2, 3, 4, 5]).pipe(max()).subscribe(log);

// 测试三
const obj = [
	{ name: "Student A", score: 80 },
	{ name: "Student B", score: 90 },
	{ name: "Student C", score: 60 },
	{ name: "Student D", score: 70 },
];
from(obj)
	.pipe(max((x, y) => x.score - y.score))
	.subscribe(log);

```


### count

* 概念：用来计算来源Observable 发生过多少次事件，也可以传入一个 predicate callback function，判断事件资料是否符合固定条件，当来源Observable 结束时，新的Observable 会发生的事件值为「所有符合指定条件事件」的总数

```typescript
interval(500).pipe(take(5), count()).subscribe(log);

interval(500)
	.pipe(
		take(5),
		count((data) => data >= 3)
	)
	.subscribe(log);

```


### reducer

* 概念：用来运算来源Observable 汇总后的结果, 与 scan 很像，可以理解为 scan 每次都会把运算只抛出来，而 reduce 只有在最后才会把值抛出来

```typescript
const data = [100, 500, 300, 250];
of(...data)
	.pipe(scan((acc, value) => acc + value, 0))
	.subscribe(log);

of(...data)
	.pipe(reduce((acc, value) => acc + value, 0))
	.subscribe(log);

```


## 总结

* min：找出来源Observable 事件的最小值。

* max：找出来源Observable 事件的最大值。

* count：找出来源Observable 事件总数。

* reduce：依照指定运算逻辑，找出来源Observable 事件汇总的结果。











