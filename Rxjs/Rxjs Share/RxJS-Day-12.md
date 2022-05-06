# ReactiveX JavaScript（RxJS）- Day 12

## RxJS 建立Observable 的基础- Observable / Subject / BehaviorSubject / ReplaySubject / AsyncSubject

### Observable

* Observable是RxJS 中建立串流最基本的方式之一，我们可以透过 Observable 类别来建立一个「可被观察的」物件，我们会在这个物件内先写好整个资料流的流程，以便未来订阅(subscribe) 时可以依照这资料流程进行处理：

1. 建立Observable：

   因为 Observable 是一个类别，所以最简单的建立方式自然是直接 new 它：

```typescript
import { Observable } from 'rxjs';
const source$ = new Observable();

```

2. 建立资料流

   使用 `Observable` 建立资料流时，可以传入一个callback function，function 内只有一个物件参数，我们称为订阅者(Subscriber)，这个订阅者就是处理资料流程的人，也就是负责呼叫`next()`、`complete()`和 `error()` 的物件，我们可以透过这个物件先设计好资料流的流程，例如发送1、2、3、4 然后结束：

```typescript
   const source$ = new Observable(subscriber => {
     console.log('stream 开始');
     subscriber.next(1);
     subscriber.next(2);
     subscriber.next(3);
     subscriber.next(4);
     console.log('stream 结束');
     subscriber.complete();
   });
```

3. 订阅Observable

```typescript
   source$.subscribe({
     next: data => console.log(`Observable 第一次订阅完成: ${data}`),
     complete:() => console.log('第一次订阅完成')
   });
   // stream 开始
   // Observable 第一次订阅完成: 1
   // Observable 第一次订阅完成: 2
   // Observable 第一次订阅完成: 3
   // Observable 第一次订阅完成: 4
   // stream 结束
   // 第一次订阅完成
```

每次订阅发生时，就会呼叫 `new Observable()` 内的callback function，以上面的例子来说，这样的呼叫是**同步**的，也就是发生两次订阅时，会等第一次订阅全部跑完，才跑第二次订阅：

4. 异步执行

```typescript
const source0$ = new Observable((subscriber) => {
	console.log("stream 开始");
	subscriber.next(1);
	subscriber.next(2);
	subscriber.next(3);
	setTimeout(() => {
		subscriber.next(4);
		subscriber.complete();
		console.log("stream 结束");
	});
});

const source$ = new Observable((subscriber) => {
	console.log("stream 开始");
	subscriber.next(1);
	subscriber.next(2);
	subscriber.next(3);
	subscriber.next(4);
	console.log("stream 结束");
	subscriber.complete();
});

source0$.subscribe({
	next: (data) => console.log(`Observable 第一次订阅完成: ${data}`),
	complete: () => console.log("第一次订阅完成"),
});

source$.subscribe({
	next: (data) => console.log(`Observable 第二次订阅完成: ${data}`),
	complete: () => console.log("第二次订阅完成"),
});

// stream 开始
// Observable 第一次订阅完成: 1
// Observable 第一次订阅完成: 2
// Observable 第一次订阅完成: 3
// stream 开始
// Observable 第二次订阅完成: 1
// Observable 第二次订阅完成: 2
// Observable 第二次订阅完成: 3
// Observable 第二次订阅完成: 4
// stream 结束
// 第二次订阅完成
// Observable 第一次订阅完成: 4
// 第一次订阅完成
// stream 结束
```

**Observable非常适合在有固定资料流程的情境，先把流程建立好，之后每次订阅都会照这个流程执行**



### Subject

* Subject系列继承了 Observable 类别，并给予了更多不同的特性，因此我们会说 Subject 也是一种Observable；而 Subject 与 Observable 有两个明显不同的地方：

不同点一：Observable在建立物件同时就决定好资料流向了，而 Subject 是在产生物件后才决定资料的流向。

不同点二：Observable每个订阅者都会得到独立的资料流，又称为unicast；而 Subject 则是每次事件发生时就会同步传递给所有订阅者(Observer)，又称为multicast。

由于 Subject 是在产生物件后才决定资料流向，因此比较适合在程式互动过程中动态决定资料流向，也就是 Subjct 建立好后，将这个 Subject 物件传出去，让其它程式来透过呼叫该物件的 next() 等方法来决定资料流向

另外，同样是订阅，Subject的订阅与Observer 的关系是一对多的，而 Observable 的订阅与Observer 则是一对一关系。

看一段代码

```typescript
const subject$ = new Subject();
subject$.subscribe((data) => console.log(`第一次订阅: ${data}`));
subject$.next(1);
subject$.next(2);
subject$.subscribe((data) => console.log(`第二次订阅: ${data}`));

subject$.next(3);
subject$.next(4);

subject$.subscribe((data) => console.log(`第三次订阅: ${data}`));
subject$.complete();
subject$.next(5);

// Subject 第一次订阅: 1
// Subject 第一次订阅: 2
// Subject 第一次订阅: 3
// Subject 第二次订阅: 3
// Subject 第一次订阅: 4
// Subject 第二次订阅: 4

```

可以看到每次订阅后，都会在有新的事件时才会收到新事件的资料。每次订阅都是直接订阅这条执行中的资料流，这就是跟 Observable 最大不同的地方。


### BehaviorSubject

* Subject产生的物件在订阅时若没有事件发生，会一直收不到资料，如果希望在一开始订阅时会先收到一个预设值，且有事件发生后才订阅的行为也可以收到最近一次发生过的事件资料，则可以使用 BehaviorSubject

这个大概理解为就是 有一个初始值会被 next 掉，然后当有新的 subscribe 时候，会收到最后一个的订阅，还有一个 value 的值 可以获取到最后的值

```typescript
const behaviorSource$ = new BehaviorSubject(0);
behaviorSource$.subscribe((data) => console.log(`第一次订阅: ${data}`));

behaviorSource$.next(1);
behaviorSource$.next(2);

behaviorSource$.subscribe((data) => console.log(`第二次订阅: ${data}`));
behaviorSource$.next(3);

console.log(`最近一次的值： ${behaviorSource$.value}`);

// 第一次订阅: 0
// 第一次订阅: 1
// 第一次订阅: 2
// 第二次订阅: 2
// 第一次订阅: 3
// 第二次订阅: 3
// 最近一次的值： 3

```

对于需要保留最近一次资料状态的情境来说，BehaviorSubject 还是很不错的


### ReplaySubject

* ReplaySubject 有「重播」的意思，ReplaySubject会帮我们保留最近N 次的事件资料，并在订阅时重播这些发生过的事件资料给订阅者，跟BehaviorSubject 类似，都有cache 的概念，只是更有弹性

可以理解为：重复最后n次数据

```typescript
// 设置缓存三个值
const replaySubject$ = new ReplaySubject(3);
replaySubject$.subscribe((data) => console.log(`第1次订阅: ${data}`));

replaySubject$.next(1);
replaySubject$.next(2);
replaySubject$.next(3);
replaySubject$.next(4);

replaySubject$.subscribe((data) => console.log(`第2次订阅: ${data}`));

replaySubject$.next(5);
replaySubject$.next(6);

replaySubject$.subscribe((data) => console.log(`第3次订阅: ${data}`));

// 当订阅不够三个的时候有几个抛出几个，当订阅够三个时候抛出三个

// 第1次订阅: 1
// 第1次订阅: 2
// 第1次订阅: 3
// 第1次订阅: 4
// 第2次订阅: 2
// 第2次订阅: 3
// 第2次订阅: 4
// 第1次订阅: 5
// 第2次订阅: 5
// 第1次订阅: 6
// 第2次订阅: 6
// 第3次訂閱: 4
// 第3次訂閱: 5
// 第3次訂閱: 6

```


### AsyncSubject

*  AsyncSubject 物件被建立后，过程中发生任何事件都不会收到资料，直到 complete() 被呼叫后，才会收到「最后一次事件资料」

```typescript
const asyncSubject$ = new AsyncSubject();

asyncSubject$.subscribe((data) => console.log(`第1次订阅: ${data}`));
asyncSubject$.next(1);
asyncSubject$.next(2);

asyncSubject$.subscribe((data) => console.log(`第2次订阅: ${data}`));
asyncSubject$.next(3);

asyncSubject$.complete();

// 第1次订阅: 3
// 第2次订阅: 3

```

如果你只希望关注 最后的资料就可以使用AsyncSubject


### 共用API - asObservable

* 所有的 Subject 系列都有一个共用且常用的API，称为 asObservable，它的用途是将 Subject 当作 Observable 回传，这样有什么好处呢？由于 Observable 并没有next()、complete()和 error() 这样的API，因此可以让得到这个 Observable 物件的方法专注在资料流订阅相关的处理就好，而不被允许发送新的事件，就可以将发送新事件等行为封装起来不被外界看到啦！

```typescript
class Student {
	private _subject$ = new Subject();

	get subject$() {
		return this._subject$.asObservable();
	}

	updateSource = (score) => {
		if (score > 50) {
			this._subject$.next(score);
			return;
		}
		this._subject$.next(50);
	};
}

const student = new Student();

student.subject$.subscribe((data) => console.log(`订阅: ${data}`));

student.updateSource(70); // 订阅: 70
student.updateSource(50); // 订阅: 50
student.updateSource(80); // 订阅: 80
// student.subject$.next(50); // (错误：Uncaught TypeError: student.subject$.next is not a function)

```



