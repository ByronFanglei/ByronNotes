# ReactiveX JavaScript（RxJS）- Day 15

* 今天我们来介绍更多建立类型的operators，分别是fromXXXX 系列，和一些跟时间操作有关的operators

## from
* 概括：接受的参数类型包含数组、可叠代的物件(iterable)、Promise 和「其他Observable 实作」 等等，from会根据传递进来的参数决定要如何建立一个新的Observable

```typescript
import { from, of } from "rxjs";
// 传递数组当参数
from([1, 2, 3, 4, 5]).subscribe((data) => console.log(data));
// 1
// 2
// 3
// 4
// 5

// 传递可迭代的物件当参数
function* NewFrom(start, end) {
	if (start > end) {
		[start, end] = [end, start];
	}
	for (let x = start; x <= end; x++) {
		yield x;
	}
}

from(NewFrom(1, 5)).subscribe((data) => console.log(data));
// 1
// 2
// 3
// 4
// 5

// 传递Promise 当参数
from(Promise.resolve(1)).subscribe((data) => console.log(data));
// 1

// 传递Observable 当参数
from(of(1, 2, 3, 4, 5)).subscribe((data) => console.log(data));
// 1
// 2
// 3
// 4
// 5

```

## fromEvent
* 概括：将浏览器事件包装成Observable， 两个参数：target监听事件的DOM 元素，eventName时间名称

```typescript
import { fromEvent } from "rxjs";

fromEvent(document, "click").subscribe((data) => console.log(data));
```

## fromEventPattern
* 概括：根据自订的逻辑决定事件发生, 两个参数，当订阅后时调用第一个参数，取消订阅调用第二个参数

```typescript
import { fromEventPattern } from "rxjs";

const addHandler = (handle) => {
	console.log("订阅了");
	document.addEventListener("click", (every) => handle(event));
};
const removeHandler = (handle) => {
	console.log("取消订阅了");
	document.removeEventListener("click", handle);
};
const source$ = fromEventPattern(addHandler, removeHandler);

const subscription = source$.subscribe((data) => {
	console.log(`fromEventPattern:`, data);
});

setTimeout(() => {
	subscription.unsubscribe();
}, 3000);

// （订阅后---3000ms---取消订阅｜）
```

## interval
* 概括：传递一个时间ms 根据设定的时间来建立 Observable，当被订阅后就会按照指定时间发生一次资料流，interval有一个缺点就是 一开始一定要等待一个时间后才会开始执行，这个缺点可以将timer的第一个参数设置成 0 即可解决

```typescript
import { interval } from "rxjs";
import { filter } from "rxjs/operators";

const subscription = interval(1000)
	.pipe(filter((_, index) => index % 2 === 0))
	.subscribe((data) => console.log(data));

setTimeout(() => {
	subscription.unsubscribe();
}, 3000);
// （---0---2｜）
```

## timer
* 概括：与interval类似，但是多一个参数代表多久时间ms后开始，然后依照指定时间ms建立Observable，如果timer没有第二个参数，那么当指定时间后 触发一次事件后就结束了

```typescript
import { timer } from "rxjs";
import { filter } from "rxjs/operators";

const subscription = timer(3000, 500)
	.pipe(filter((_, index) => index % 2 === 0))
	.subscribe((data) => console.log(data));

setTimeout(() => {
	subscription.unsubscribe();
}, 5000);
// （---3000ms---0--2--|）
```

## defer
* 概括：传入一个 fun/Promise 参数， 当 defer 建立的 Observable 被订阅时，会呼叫这个参数，并以里面回传的Observer 当作资料流，主要目的是为了做到延迟执行

```typescript
import { defer } from "rxjs";

// 通常情况下使用 Promise
const p = new Promise((resolve) => {
	console.log("Promise 被执行了！"); // 在创建 Promise 的时候就会被执行
	setTimeout(() => {
		resolve(1);
	}, 3000);
});

p.then((data) => {
	console.log(data);
});

// 当我们想只有在订阅的时候再去调用 Promise 就可以使用 defer
const pp = () => {
	return new Promise((resolve) => {
		console.log("Promise 被执行了！"); // 只有在订阅的时候 才会被执行
		setTimeout(() => {
			resolve(1);
		}, 3000);
	});
};

const source$ = defer(pp);

source$.subscribe((data) => console.log(data));
```


## 总结
1. from：使用数组、Promoise、Observable 等来源建立新的 Observable。
2. fromEvent：封装DOM 的 addEventListener 事件处理来建立 Observable。
3. fromEvenPattern：可依照自行定义的事件来建立 Observable。
4. interval：每隔指定的时间发出一次事件值。
5. timer：与 interval 相同，但可以设定起始的等待时间。
6. defer：用来延迟执行内部的 Observable。