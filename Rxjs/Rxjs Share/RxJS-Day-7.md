# ReactiveX JavaScript（RxJS）- Day 7

### 解析 RxJS 实现观察者模式代码

```typescript
// RxJS 实现观察者模式

import { Subject } from "rxjs";

// 建立被观察目标，之后需要 subject$.next() 将通知发送给观察者
let subject$: Subject<string> = new Subject();

// 建立第一个观察者
// 观察者A
const observeA = {
  // 成功订阅
	next: (data) => console.log(`我是 observeA 观察到了 ${data}`),
  // 完成
	complete: () => {},
  // 错误
	error: (err) => console.log(`我是 observeA 订阅出错了 ${err}`),
};

// 观察者B
const observeB = {
	next: (data) => console.log(`我是 observeB 观察到了 ${data}`),
	complete: () => {},
	error: (err) => console.log(`我是 observeB 订阅出错了 ${err}`),
};

// 执行订阅(加入通知对象)，以观察者模式来说，我们会说「把观察者加入被通知的对象清单」；而在RxJS 中，我更喜欢说成「订阅某个目标，把资料交给观察者处理」
const subscribeA$ = subject$.subscribe(observeA);
// 发布通知
subject$.next("蜘蛛侠");
// 当然我们在加入订阅的时候如果用不到 complete 或者 error，我们可以直接这样写
const subscribeB$ = subject$.subscribe(data => {
  console.log(`我是 observeB 观察到了 ${data}`)
});
subject$.next("钢铁侠");
// 取消订阅
subscribeA$.unsubscribe();
subject$.next("纸钞屋");

// 我是 observeA 观察到了 蜘蛛侠
// 我是 observeA 观察到了 钢铁侠
// 我是 observeB 观察到了 钢铁侠
// 我是 observeB 观察到了 纸钞屋

```