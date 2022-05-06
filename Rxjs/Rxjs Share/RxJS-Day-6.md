# ReactiveX JavaScript（RxJS）- Day 6

## 关于观察者模式

* 概念：一个目标物件管理所有相依于它的观察者物件，并且在它本身的状态改变时主动发出通知。这通常透过呼叫各观察者所提供的方法来实现。此种模式通常被用来实时事件处理系统。

* 也可以这样理解：「当我们要观察一个资料的变化时，与其主动去关注它的状态，不如由观察的目标主动告知我们资料变化，可以更加即时且可靠的得知资料变化。」

* 观察者模式解决的问题：比如我们使用 bilibili 订阅某个 up 主，当这个 up 主有发出一个新内容时。我们就会收到这个 up 主的推送，这个就使用到了 观察者模式

## 观察者模式定义的角色
* 观察者(Observer)
	*	notify： 当目标资料变更，会呼叫观察者的notify 方法，告知资料变更了

* 目标(Subject)
	*	notifyObservers：用来通知所有目前的观察者资料变更了，也就是呼叫所有观察者物件的 notify 方法
	*	addObserver：将某个物件加入观察者清单
	*	deleteObserver：将某个物件从观察者清单中移除


```typescript
// js 实现观察者模式

// 观察者A
const observeA = {
	notify: (data) => {
		console.log(`我是 observeA 观察到了 ${data}`);
	},
};

// 观察者B
const observeB = {
	notify: (data) => {
		console.log(`我是 observeB 观察到了 ${data}`);
	},
};

class Subject {
	observes = [];
	notifyObservers = (data) => {
		this.observes.forEach((item) => {
			item?.notify(data);
		});
	};

	addObserve = (newObs) => {
		this.observes.push(newObs);
	};

	delectObserve = (obs) => {
		this.observes = this.observes.filter((item) => item !== obs);
	};
}

const subject = new Subject();

subject.notifyObservers("蜘蛛侠");
subject.addObserve(observeA);
subject.notifyObservers("奇异博士");
subject.addObserve(observeB);
subject.notifyObservers("钢铁侠");
subject.delectObserve(observeA);
subject.notifyObservers("绿巨人");

// 我是 observeA 观察到了 奇异博士
// 我是 observeA 观察到了 钢铁侠
// 我是 observeB 观察到了 钢铁侠
// 我是 observeB 观察到了 绿巨人
```


```typescript
// RxJS 实现观察者模式

import { Subject } from "rxjs";

let subject$: Subject<string> = new Subject();

// 观察者A
const observeA = {
	next: (data) => console.log(`我是 observeA 观察到了 ${data}`),
	complete: () => {},
	error: (err) => console.log(`我是 observeA 订阅出错了 ${err}`),
};

// 观察者B
const observeB = {
	next: (data) => console.log(`我是 observeB 观察到了 ${data}`),
	complete: () => {},
	error: (err) => console.log(`我是 observeB 订阅出错了 ${err}`),
};

const subscribeA$ = subject$.subscribe(observeA);
subject$.next("蜘蛛侠");
const subscribeB$ = subject$.subscribe(observeB);
subject$.next("钢铁侠");
subscribeA$.unsubscribe();
subject$.next("纸钞屋");

// 我是 observeA 观察到了 蜘蛛侠
// 我是 observeA 观察到了 钢铁侠
// 我是 observeB 观察到了 钢铁侠
// 我是 observeB 观察到了 纸钞屋

```