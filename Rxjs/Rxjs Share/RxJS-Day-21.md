# ReactiveX JavaScript（RxJS）- Day 21

## RxJS 过滤类型Operators 

* filter / first / last / single

```typescript
// 公共代码
import { EMPTY, timer } from "rxjs";
import { filter, take, first, last, single } from "rxjs/operators";

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

const source$ = timer(0, 1000).pipe(take(10));

```

### filter

* 概念：跟数组的 filter 是一样的用法，这没啥好说的

```typescript
source$.pipe(filter((item, index) => item >= 3)).subscribe(log);

```

### first

* 概念：获取第一个结果，然后结束，当 first 内部设定一个函数时候，则返回第一次符合条件的值

```typescript
source$.pipe(first()).subscribe(log);
source$.pipe(first((data) => data >= 3)).subscribe(log);

```

### last

* 概念：last跟 first 相反，last是取整个来源资料流「最后一次发生的事件」，因此原来的资料流一定要有「结束」(complete) 发生

```typescript
source$.pipe(last()).subscribe(log);
source$.pipe(last((data) => data < 9)).subscribe(log);

```

### single

* 概念：single比较特殊，它可以帮助我们「限制」整个资料流只会有一次事件发生，当发生第二次事件时，就会发生错， 如果写了 complete 方法就会调用 complete 不会发生错误

```typescript
source$.pipe(last()).subscribe(log);
source$.pipe(last((data) => data < 9)).subscribe(log);

// 当整个资料流没有数据返回，也算是不符合「发生一次事件」的条件，因此一样会发生错误
EMPTY.pipe(single()).subscribe(log);

// single也一样可以传入callback function，此时条件会变成「在条件符合时，如果整个资料流只发生过一次事件，发生该事件的值，否则发生undefined，然后结束」
source$.pipe(single((data) => data === 0)).subscribe(log);

```


## 总结

* filter：用来依照指定的条件过滤事件值，只有符合条件的事件值会发生。

* first：只有第一个事件值会发生，若有指定条件会变成符合条件的第一个事件值会发生。

* last：只有最后一个事件值会发生，若有指定条件会变成符合条件的最后一个事件值会发生。

* single：可以用来确保整个Observable 「只会发生一次事件」，没有指定条件时，发生两次以上事件会发生错误；有指定条件时，发生两次以上事件会产生 undefined 事件值。

