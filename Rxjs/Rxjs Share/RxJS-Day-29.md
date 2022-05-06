# ReactiveX JavaScript（RxJS）- Day 29

## RxJS 错误处理Operators

* catchError / finalize / retry / retryWhen

```typescript
import { fromEvent, iif, interval, of, throwError } from "rxjs";
import {
	catchError,
	finalize,
	map,
	retry,
	retryWhen,
	switchMap,
	take,
	tap,
} from "rxjs/operators";

const log = {
	next: (data) => {
		console.log(data);
	},
	error: (err) => {
		console.log("err:", err);
	},
	complete: () => {
		console.log("complete");
	},
};

```


###  catchError

* 概念：可以在来源Observable 发生错误时，进行额外的处理，一般来说错误在订阅的时候进行错误处理，catchError可以在 Observable 数据传递期间可以拦截错误做一些处理

```typescript
interval(500)
	.pipe(
		map((data) => {
			if (data % 2 === 0) {
				return data;
			} else {
				throw new Error(`error: ${data}`);
			}
		}),
		catchError((err) => {
			// 这里可以根据错误的情况来判断是要抛出错误还是继续向下进行
			// return interval(1000);
			// throw new Error(`error: ${err}`);
			return throwError(err);
		})
	)
	.subscribe(log);


```

###  retry

* 概念：当Observable 发生错误时，可以使用 retry 来重试整个Observable，在 retry 内可以指定重试几次
若不指定次数，预设为-1，代表会持续重试；若不想重试，也可以指定次数为0，就会直接让错误发生。

```typescript
interval(500)
	.pipe(
		switchMap((data) => iif(() => data === 3, throwError(data), of(data))),
		retry(3)
	)
	.subscribe(log);

```


### retryWhen

* 概念：可以再发生错误时进行重试，但 retryWhen 更有弹性，在 retryWhen 内需要设计一个notifiercallback function，retryWhen会将错误资讯传入notifierfunction，同时需要回传一个Observable，retryWhen会订阅这个Observable，每当有事件发生时，就进行重试，直到这个回传的Observable 结束，才停止重试

```typescript
// 当发生错误后，没过三秒重试一次，试三次
interval(500)
	.pipe(
		switchMap((data) => iif(() => data === 3, throwError(data), of(data))),
		retryWhen((error) =>
			interval(3000).pipe(
				take(3)
				// tap(() => {
				// 做一些副作用操作
				// 	console.log("error:", error);
				// })
			)
		)
	)
	.subscribe(log);


// 每300ms发出一个值，等于3抛出错误，如果重试五次后还抛错 就发生错误
interval(300)
	.pipe(
		switchMap((data) => iif(() => data === 3, throwError("err"), of(data))),
		retryWhen((err) => {
			return interval(3000).pipe(
				switchMap((data, index) => {
					return iif(
						() => index === 3,
						throwError("重试五次了，报错了"),
						of(data)
					);
				})
			);
		})
	)
	.subscribe(log);

// 每300ms发出一个值，等于3抛出错误，当用户点击的时候进行重试， 重试 3 次 后抛出错误
const click$ = fromEvent(document, "click");
interval(300)
	.pipe(
		switchMap((data) => iif(() => data === 3, throwError("err"), of(data))),
		retryWhen((err) => click$)
	)
	.subscribe(log);

```


### finalize

* 概念：在整个来源Observable 结束时，才进入处理，因此永远会在最后才呼叫到
严格来说 finalize 不算是错误处理的operator，因为 finalize 会在整个Observable 结束时才进入处理，跟有没有发生错误无关，但经常与错误处理搭配一起使用

```typescript
interval(500)
	.pipe(
		take(5),
		switchMap((data) => iif(() => data === 3, throwError("err"), of(data))),
		finalize(() => console.log("全部都结束了！！")),
		map((data) => data + 1)
	)
	.subscribe(log);

// finalize也会比 subsribe 的 complete、error 抛出的还慢，透过 finalize 我们可以确保就算过程中发生错误导致整个资料流中断，还会有个地方可以处理些事情

```


## 总结

* catchError：可以用来决定当来源Observable 发生错误时该如何进行，回传一个Observable 代表会使用此Observable 继续下去，因此回传 throwError 则代表依然发生错误。

* retry：当来源Observable 发生错误时，重新尝试指定次数。

* retryWhen：当来源Observable 发生错误时，可以照自定的Observable 来决定重试的时机。

* finalize：在Observable 结束时，无论是 error() 还是complete()，最后都可以进入 finalize 进行最终处理。
