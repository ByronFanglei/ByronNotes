# ReactiveX JavaScript（RxJS）- Day 4

## 实战练习

### 需求：
1. 画面必须显示三个资讯：
  *	目前状态：包含「开始计数」、「完成」和「错误」(包含错误讯息)。
  *	目前计数：当计数器建立后，显示「计数」按钮被点击的次数。
  *	偶数计数：每当「目前计数」数值为偶数时，显示这个偶数值。
2. 画面包含四个按钮，功能如下：
  *	「开始新的计数器」按钮：重新建立一个新的计数器，必须使用 **new Subject()** 来建立；并在 「目前状态」资讯显示「开始计数」。
  *	「计数」按钮：当建立新的计数器后，每按下计数按钮，显示的计数值就加1。
  *	「发生错误」按钮：要求使用者输入错误讯息，并将错误讯息显示在「目前状态」资讯内。
  *	「完成计数」按钮：在「目前状态」资讯显示「完成」
3. 其他要求：
  *	当按下「开始新的计数器」时，所有计数器归0。
  *	当按下「发生错误」或「完成计数」，除非按下「开始新的计数器」，否则其他按钮按下不会有任何动作。


### 编码

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>RxJS Practice</title>
	</head>
	<body>
		<button id="init">开始新的计数器</button>
		<button id="start">计数</button>
		<button id="err">发生错误</button>
		<button id="complete">完成计数</button>
		<div id="texts" style="display: none">
			<div>目前状态：<span id="status">完成</span></div>
			<div>目前计数：<span id="current">0</span></div>
			<div>偶数计数：<span id="odd">0</span></div>
		</div>

		<script src="./src/index.ts"></script>
	</body>
</html>

```

```typescript
import { fromEvent, Subject } from "rxjs";
import { filter } from "rxjs/operators";

// 获取 DOM
const init: HTMLElement = document.querySelector("#init");
const Start: HTMLElement = document.querySelector("#start");
const Err: HTMLElement = document.querySelector("#err");
const Complete: HTMLElement = document.querySelector("#complete");
const Texts: HTMLElement = document.querySelector("#texts");
const Status: HTMLElement = document.querySelector("#status");
const Current: HTMLElement = document.querySelector("#current");
const Odd: HTMLElement = document.querySelector("#odd");

let subject$: Subject<number>;
let Count = 1;

// 定义 observable
const observable = {
	next: (data) => console.log(data),
	complete: () => {
		Status.innerText = "END";
	},
	error: (err) => {
		Status.innerText = `发生错误：${err}`;
	},
};

fromEvent(init, "click").subscribe(() => {
	subject$ = new Subject();
	Count = 0;
	Texts.style.display = "block";

	Status.innerText = "开始计数---";
	subject$.subscribe(() => {
		Current.innerText = String(Count);
	});

	// 处理偶数
	const evenCounter = subject$.pipe(filter((_) => _ % 2 === 0));
	evenCounter.subscribe(() => {
		Odd.innerText = String(Count);
	});

	subject$.subscribe(observable);

	subject$.next(Count);
});

// 处理计数
fromEvent(Start, "click").subscribe(() => {
	if (subject$) {
		subject$.next(++Count);
	}
});

// 处理完成
fromEvent(Complete, "click").subscribe(() => {
	if (subject$) {
		subject$.complete();
	}
});

// 处理开始
fromEvent(Err, "click").subscribe(() => {
	if (subject$) {
		subject$.error("ERROR");
	}
});

```

### 验证需求即可