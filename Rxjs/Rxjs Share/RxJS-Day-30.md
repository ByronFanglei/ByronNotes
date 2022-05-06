# ReactiveX JavaScript（RxJS）- Day 30

## RxJS Multicast 类Operator

* multicast / publish / refCount / share / shareReplay

```typescript
import { interval, Subject, ConnectableObservable, AsyncSubject } from "rxjs";
import {
	multicast,
	take,
	map,
	publish,
	refCount,
	share,
	shareReplay,
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


### multicast

* 概念：Cold Observable 每次订阅只会对应一个观察者，因此也可以说成将资料播放(cast) 给唯一的观察者，应此也称为单播(unicast)，而 multicast 就是来源Observable 变成多播(multicast)的情况

```typescript
const source$ = interval(1000).pipe(
	take(10),
	multicast(() => new Subject())
	// AsyncSubject 只打印对后一次的值
	// multicast(() => new AsyncSubject())
);
(source$ as ConnectableObservable<any>).connect();

source$.subscribe((data) => {
	console.log(`multicast 第一次订阅: ${data}`);
});

setTimeout(() => {
	source$.subscribe((data) => {
		console.log(`multicast 第二次订阅: ${data}`);
	});
}, 5000);

setTimeout(() => {
	// 使用 TypeScript 转型成 ConnectableObservable
	// 使用 JavaScript 直接调用 connect() 就好
	(source$ as ConnectableObservable<any>).connect();
}, 3000);

```

第二个参数是一个 selector callback function, 这个selectorfunction 会收到被建立的Subject 类别，同时回传另一个Observable，当使用这个参数时，将不再会对来源Observable 进行多次订阅，变成每次订阅都会重新建立新的Subject 并加上selector function 回传的Observable 进行订阅；也因此新的Observable 不再是ConnectableObservable，也就不用再次呼叫connect()

```typescript
const source2$ = interval(1000).pipe(
	take(5),
	multicast(
		() => new Subject(),
		(subject) => subject.pipe(map((data: number) => data + 1))
	)
);

source2$.subscribe((data) => {
	console.log(`multicast 第一次订阅: ${data}`);
});

setTimeout(() => {
	source2$.subscribe((data) => {
		console.log(`multicast 第二次订阅: ${data}`);
	});
}, 3000);
// 上面代码中，每次订阅发生时，会使用 new Subject() 产出的新Subject 类别做为多播的来源，以及搭配selectorfunction 回传的Observable 订阅，并多播给每次订阅的观察者，由于是使用Subject 类别，因此订阅来源依然是多播的Observable，只是这个Observable 只会有目前订阅的观察者收到而已。

```


### publish

* 概念：publish将 multicast 内封装了 multicast 内建立Subject 的方法，直接使用new Subject()

```typescript
// 以下两段代码完全一样，有兴趣可以看看publish的源码
interval(1000).pipe(
  multicast(() => new Subject())
);

interval(1000).pipe(
  publish()
);

```

* publishLast：等于multicast(() => new AsyncSubject())

* publishBehavior：等于multicast(() => new BehaviorSubject())，使用的参数与 BehaviorSubject 相同

* publishReplay：等于multicast(() => new ReplaySubject())，使用的参数与 ReplaySubject 相同


### refCount

* 概念：当Observable 是Connectable Observable 时，我们必须主动呼叫connect，才可以让资料开始流动(当然也要有订阅发生)，如果不需要自行控制 connect 时机，可以使用 refCount 来帮我们呼叫connect

```typescript
const source1$ = interval(500)
	.pipe(take(5), publish())
	.subscribe((data) => console.log(`source1$: ${data}`));

const source2$ = interval(500)
	.pipe(take(5), publish(), refCount())
	.subscribe((data) => console.log(`source2$: ${data}`));

//从执行结果可以看到，source1$因为没有主动去呼叫 connect() 的关系，虽然有订阅，但还没办法开始；而 source2$ 则使用 refCount() 帮我们呼叫connect()，因此当订阅发生时，整个资料流就会直接开始

```


### share

* 概念：相当与multicast(new Subject()) 与 refCount() 的组合 或者说 publish() 与 refCount() 的组合

```typescript
const source$ = interval(500).pipe(take(5), share()).subscribe(log);

```


### shareReplay

* 概念：shareReplay可以直接当作 multicast(new ReplaySubject()) 与 refCount() 的组合，与 share() 不同的地方在于，shareReplay()还有重播的概念，也就是每次订阅时，会重播过去N 次发生的资料

```typescript
const source$ = interval(500).pipe(take(20), shareReplay(2));

source$.subscribe((data) => console.log(`first: ${data}`));

setTimeout(() => {
	source$.subscribe((data) => console.log(`second: ${data}`));
}, 5000);

```


## 总结

* multicast：将单播(unicast) 的Observable 转换成多播(multicast)，需要决定使用哪种多播的来源( Subject、BehaviorSubject等等)，之后会得到一个ConnectableObservable，需要呼叫它的 connect() 方法后才能开始资料流。可自行决定 connect() 时机。

* publish：multicast的特定版本，直接使用 Subject 类别做为多播的来源。
  等同于multicast(() => new Subject())
  另外还有：

  > publishLast：multicast(() => new AsyncSubject())

  > publishBehavior：multicast(() => new BehaviorSubject())

  > publishReplay：multicast(() => new ReplaySubject())

* refCount：帮我们直接呼叫来源ConnectableObservable 的 connect() 方法。

* share：意义为来源Observable 的资料共享给所有观察者。

  > multicast(() => new Subject())+refCount()

* shareReplay：每次订阅时会重播来源Observable 最近N 次的资料，也就是最近N 次事件资料共享给所有观察者。

  > multicast(() => new ReplaySubject())+refCount()



