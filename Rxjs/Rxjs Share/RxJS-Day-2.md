# ReactiveX JavaScript（RxJS）- Day 2

## 准备环境

### 使用 CDN 引入或者下载 library 后加载
* RxJS CDN：https://unpkg.com/@reactivex/rxjs@6.0.0/dist/global/rxjs.umd.js

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>RxJS</title>
	</head>
	<body>
		<script src="https://unpkg.com/@reactivex/rxjs@6.0.0/dist/global/rxjs.umd.js"></script>
		<script>
		// 当点击次数是偶数时展示当前坐标
			rxjs
				.fromEvent(document, "click")
				.pipe(
					rxjs.operators.filter((_, index) => {
						if (index % 2 === 0) {
							Object.assign(_, { currentIndex: index });
							return true;
						}
						return false;
					}),
					rxjs.operators.map((event) => ({
						x: event.x,
						y: event.y,
						index: event?.currentIndex,
					}))
				)
				.subscribe((data) =>
					console.log(
						`x: ${data?.x}, y: ${data?.y}, 当前点击次数: ${data?.index}`
					)
				);
		</script>
	</body>
</html>

```

直接引入 CDN 会在有点慢，而且不符合先在的开发标准版，接下来我们用 Parcel 来试试


### 使用Parcel （推荐）

* Parcel：https://parceljs.org/

​	直接使用CDN 载入的方式虽然非常简单，但也有不少缺点，像是一次要载入所有RxJS 的程式码所以载入速度比较慢，每次使用都要加上 rxjs. 开头有点麻烦；纯JavaScript 也不太方便，如果可以用TypeScript 写起来会更轻松。

​	比较现代化的做法会使用ES 6 的import / export 语法，只载入需要的部分，语法上会更精简，之后再搭配webpack 或babel 等工具，把不需要的程式码都移除(我们称为tree shaking)，加快载入速度，有使用TypeScript 的话，也能自动帮我们转换成JavaScript。

​	若没有很复杂的需求，推荐可以使用Parcel这个工具，可以几乎不用做任何设置就完成上述的需求！以下是使用Parcel 建立RxJS 练习环境的步骤：

1. 安装 nodejs、npm，安装完 node 一般来说 npm 也会 自动安装上
2. 安装 Parcel 依赖

```shell
npm install -g parcel-bundler
```

3. 建立新的目录，进行初始化

```shell
npm init
npm install --save rxjs
touch index.html
touch index.ts
```

4. index.ts 下编写代码

```typescript
import { fromEvent } from "rxjs";
import { filter, map } from "rxjs/operators";

fromEvent(document, "click")
	.pipe(
		filter((_: Event, index: number) => {
			if (index % 2 === 0) {
				Object.assign(_, { currentIndex: index });
				return true;
			}
			return false;
		}),
		map((event) => ({
			x: event.x,
			y: event.y,
			index: event?.currentIndex,
		}))
	)
	.subscribe((data) =>
		console.log(`x: ${data?.x}, y: ${data?.y}, 当前点击次数: ${data?.index}`)
	);

```

5. 挂载到 index.html 上

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>RxJS Practice</title>
  </head>
  <body>
    <script src="./index.ts"></script>
  </body>
</html>
```

6. 终端执行 parcel：parcel index.html ，即可在 http://localhost:1234 下访问刚才编写的代码