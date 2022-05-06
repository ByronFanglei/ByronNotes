# ReactiveX JavaScript（RxJS）- Day 18

## 各种「转换」类型的operators

### map

* 概括：把Observable 每次「事件的值」换成「另外一个值」，类似于数组的 map

```typescript
import { interval } from "rxjs";
import { map } from "rxjs/operators";

const subscript$ = interval(1000)
	.pipe(map((data, index) => ({ data, index })))
	.subscribe(({ data, index }) =>
		console.log(`{data: ${data}, 下标: ${index}}}`)
	);

setTimeout(() => {
	subscript$.unsubscribe();
}, 10000);

```

* Observable 的 map 是每次有事件发生时进行转换。
* 数组的 map 会立刻把整个了数组的资料劲行转换

* 例子：由于某次考试难度太高，老师决定将考试成绩开根号后乘以十后，小数点省略，再显示大于等于60 分及格，小于60 分不及格。

```typescript
import { of } from "rxjs";
import { map } from "rxjs/operators";

const studentScore = [
	{ name: "小明", score: 100 },
	{ name: "小王", score: 49 },
	{ name: "小李", score: 30 },
];

of(...studentScore)
	.pipe(
		map((data) => {
			return {
				...data,
				newScore: Math.sqrt(data.score),
			};
		}),
		map((data) => {
			return {
				...data,
				newScore: data.newScore * 10,
			};
		}),
		map((data) => {
			return {
				...data,
				newScore: Math.ceil(data.newScore),
			};
		}),
		map((data) => {
			return {
				...data,
				isPass: data.newScore >= 60,
			};
		})
	)
	.subscribe((data) => console.log(data));
	
// 以上写法用的 函数式编程 的写法，我们将执行步骤尽量拆小，让关注点更加明确，而在 map 内我们也应用了immutable (不可变物件) 的处理方式，确保每次修改都是回传一个新的物件，而不会改到原来的物件，让程式运作过程更可靠
```


### scan

* 概括 两个参数：类似与数组的 reduce
  * 累加函数：这个函数被呼叫时会传入三个参数，可以搭配这三个参数处理资料后回传一个累加结果，函数参数包含，acc：目前的累加值，也就是上一次呼叫累加函数时回传的结果， value：目前事件值，index：目前事件index
  * 初始值


```typescript
import { interval } from "rxjs";
import { scan } from "rxjs/operators";

const accumDonate$ = interval(1000)
	.pipe(
		scan(
			(acc, value, index) => acc + value, // 累加函数
			0 // 初始值
		)
	)
	.subscribe((data) => console.log(data));

setTimeout(() => {
	accumDonate$.unsubscribe();
}, 10000);
```


### pairwise

* 概括：将Observable 的事件资料「成双成对」的输出，这个operator 没有任何参数，因为他只需要Observable 作为资料来源就足够了
* pairwise会将「目前事件资料」和上一次「事件资料」组成一个长度2 的阵列，值得注意的是，因为「第一次」事件发生时，没有「上一次」事件，因此输出结果的数量永远会比总是件数量少一次

```typescript
import { of } from "rxjs";
import { pairwise } from "rxjs/operators";

of(1, 2, 3, 4, 5, 6)
	.pipe(pairwise())
	.subscribe((data) => console.log(data));
	
```


## 实战： 股价资讯提示
* 需求：假设有一个资料流会发送每日收盘时股价，平均股价约100 元上下第一天股价一定是100 元，可忽略它，从第二天开始呈现以下资讯
	* 当股价比前一天高，显示「股价上涨了！」
	* 当股价比前一天低，显示「股价下跌了！」
	* 每天提示从历史以来股价小于100 元的天数


```typescript
// 方法1
import { of } from "rxjs";
import { map, pairwise } from "rxjs/operators";

let histerNumber = 0;
const priceHistories = [100, 98, 96, 102, 99, 105, 105];

of(...priceHistories)
	.pipe(
		pairwise(),
		map((data, index) => {
			return {
				index: index + 2,
				curent: data[1],
				curentRose: data[1] - data[0],
				histerNumber: data[1] < 100 ? ++histerNumber : histerNumber,
			};
		})
	)
	.subscribe((data) => {
		const { index, curent, curentRose, histerNumber } = data;
		console.log(
			`
      第 ${index} 天
      当日股价 ${curent}
      当日股价 ${curentRose === 0 ? "持平" : curentRose > 0 ? "上涨" : "下跌"}
      历史股价低于100天数 ${histerNumber}
      `
		);
	});
```


```typescript
// 方法2
import { from } from "rxjs";
import { map, scan, pairwise } from "rxjs/operators";

const priceHistories = [100, 98, 96, 102, 99, 105, 105];

from(priceHistories)
	.pipe(
		pairwise(),
		map(([yesterdayPrice, todayPrice], index) => {
			return {
				index: index + 2,
				todayPrice,
				priceUp: todayPrice > yesterdayPrice,
				priceDown: todayPrice < yesterdayPrice,
			};
		}),
		scan(
			(acc, value) => {
				const { index, todayPrice, priceUp, priceDown } = value;
				return {
					day: index,
					todayPrice,
					priceUp,
					priceDown,
					priceBelow100Days:
						acc?.priceBelow100Days + (value?.todayPrice < 100 ? 1 : 0),
				};
			},
			{
				day: 1,
				todayPrice: 0,
				priceUp: false,
				priceDown: false,
				priceBelow100Days: 0,
			}
		)
	)
	.subscribe((data) => {
		const { day, todayPrice, priceUp, priceDown, priceBelow100Days } = data;
		console.log(
			`
      第 ${day} 天
      当日股价 ${todayPrice}
      当日股价 ${priceUp ? "上涨" : priceDown ? "下跌" : "持平"}
      历史股价低于100天数 ${priceBelow100Days}
      `
		);
	});

```





