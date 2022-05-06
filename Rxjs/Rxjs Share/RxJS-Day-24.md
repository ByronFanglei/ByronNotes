# ReactiveX JavaScript（RxJS）- Day 24

## RxJS 过滤类型Operators

* distinct / distinctUntilChanged / distinctUntilKeyChanged

```typescript
// 公共代码
import { from, Subject } from "rxjs";
import {
	distinct,
	distinctUntilChanged,
	distinctUntilKeyChanged,
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


### distinct

* 概念：将Observable 内重复的值过滤掉

```typescript
from([1, 2, 3, 4, 5, 1, 2, 3]).pipe(distinct()).subscribe(log);

// 如果 传入的是对象 可以在 distinct 内传递一个方法进行判断
const students = [
	{ id: 1, score: 70 },
	{ id: 2, score: 80 },
	{ id: 3, score: 90 },
	{ id: 1, score: 100 },
	{ id: 2, score: 100 },
];
from(students)
	.pipe(distinct((data) => data.id))
	.subscribe(log);

```

distinct内部会记录所有发生过的事件值，我们也可以透过再多传入一个Observable 的方式(参数名称为flushes)来帮助我们判断何时要清空记录的事件值的内容，每当这个Observable 有新事件发生时，就会清空来源Observable 内用来记录资料重复的物件

```typescript
const source$ = new Subject<{ id: number; score: number }>();
const sourceFlushes$ = new Subject();

source$.pipe(distinct((data) => data.id, sourceFlushes$)).subscribe(log);

setTimeout(() => source$.next({ id: 1, score: 70 }), 0);
setTimeout(() => source$.next({ id: 2, score: 80 }), 1000);
setTimeout(() => source$.next({ id: 3, score: 90 }), 2000);
setTimeout(() => source$.next({ id: 1, score: 170 }), 2200);
// 调用 sourceFlushes$ 清空数据源，重新计算
setTimeout(() => sourceFlushes$.next("sourceFlushes"), 2500);
setTimeout(() => source$.next({ id: 1, score: 70 }), 3000);

```


### distinctUntilChanged

* 概念：会过滤掉重复的事件值，直到事件资料变更为止, 就是当本次事件跟上一次一样就不会发生，否则才会发生

```typescript
from([1, 1, 2, 2, 3, 3, 4, 5, 5, 6, 7])
	.pipe(distinctUntilChanged())
	.subscribe(log);

// 如果传递的是对象，distinctUntilChanged内可以传入一个compare callback function，这个function 会传入「目前」和「上次」的事件值，让我们可以比较判断是否有被变更。
const students = [
	{ id: 1, score: 70 },
	{ id: 1, score: 80 },
	{ id: 2, score: 70 },
	{ id: 3, score: 100 },
];

from(students)
	.pipe(
		distinctUntilChanged((a, b) => {
			return a.id === b.id;
		})
	)
	.subscribe(log);
	
```

除此之外，distinctUntilChanged还有第二个参数是keySelectorfunction，这个function 跟 distinct 的 keySelector 参数一样，是用来决定传入的物件比较是否重复用的key：

```typescript
from(students)
	.pipe(
		distinctUntilChanged(
			(a, b) => a === b,
			(student) => student.id
		)
	)
	.subscribe(log);
	
// 执行结果与之前完全一样，但好处是我们把「决定比较的key」和「实际比较逻辑」拆成两个function 了，整体阅读上会更加容易
```


### distinctUntilKeyChanged：

* 概念：distinctUntilKeyChanged跟 distinctUntilChanged 基本上非常相似，但特别适合用在物件的某一个属性就是比较用的关键值(key) 的状况，distinctUntilKeyChanged的第一个参数就是事件物件的关键key 值，除此之外，distinctUntilKeyChanged还可以在再传入一个compare function，来决定资料是否重复，写个例子就明白了

```typescript
from(students)
	.pipe(distinctUntilKeyChanged("id")
	.subscribe(log);
	
```

除此之外，distinctUntilKeyChanged还可以在再传入一个compare function，来决定资料是否重复：

```typescript
from(students)
	.pipe(distinctUntilKeyChanged("id", (a, b) => a === b))
	.subscribe(log);
	
```





## 总结

* distinct：用来过滤「重复」的事件值发生，`distinct`会把出现过的事件值记录下来，当事件资料曾经出现过，就不让事件发生，我们也可以自己决定何时要把这个纪录清除。

* distinctUntilChanged：如果事件资料「持续重复」就会被过滤掉，直到这次事件资料与上次事件资料不同时，才允许事件发生。

* distinctUntilKeyChanged：与 `distinctUntilChanged` 逻辑一样，但提供了比较简单的方式，让我们处理事件物件的某个属性就是key 值的情境。







