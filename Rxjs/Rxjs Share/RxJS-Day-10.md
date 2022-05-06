# ReactiveX JavaScript（RxJS）- Day 10

## Functional Programming 常用基本技巧及应用与RxJS 应用

### 柯里化 Curry Function

* 首先 function 本身可以当作另一个 function 的参数传入，同时也能够回传一个新的 function。

* 透过这种方是，我们可以很轻易的把「设定」和「资料」进行隔离！而这种function 我们就称为Curry Function。

* 举一个例子：两个数字相加：

```typescript
const add = (a, b) => a + b;

```

我们可以重新解成a 是「设定」，代表的是要「加多少」；而b 是「资料」，代表的是「即将被增加的数」，换个好读的写法：

```typescript
const add = (num, data) => num + data;

```

接着可以把原来两数相加的程式改成这样：

```typescript
const add = (num) => {
  return (data) => num + data;
};

```

上面程式我们先建立了一个包含参数num 的 function，这个 function 会回传另一个 function，而num 在这里就只是个设定的参数，我们可以用这个function 建立更多不同功能的function

```typescript
const plusOne = add(1); // 一个加 1 的方法
const plusTwo = add(2); 

```

拿到这特定功能的function 后，只要把「资料」带进去就可以得到想要的结果啰：

```typescript
const data = 1;
const plusOneResult = plusOne(data); // 2
const plusTwoResult = plusTwo(data); // 3

```

是不是很酷啊！这种将 function 内的每个参数设定拆成回传下一个参数设定的 function 的行为，我们称为「currying」「科里化」。而最终产生的这种 function 就称为「curry function」。

透过 curry function，我们可以把一些参数设定先拆出来，但不处理资料运算；而传入不同的设定参数后取得不同目标的 function，把 function 功能拆分得更小，也让程式的复用性更高。

另外 Curry function 有一个非常大的作用，称为惰性求值 (lazy evaluation)，或称为延迟计算。Lazy evaluation 的重点在于「尽可能延后进行复杂运算的行为」！以上面的例子来说，我们可以透过  add(n)  这种方法需要运算的 function 先行准备好(此时还没真的进行运算)，直到需要的时候，才将资料带入运算。

```typescript
// 先准备好运算 (此时还没有真正的计算)
const generateNextId = add(1);
// 这里有一些其他行为
// ...

// 在真正需要的時候，才进行运算
const nextId = generateNextId(data);

```



### 实际运用

* 用昨天的偶数平方和来举个例子，如果还想另外计算「立方」该怎么办呢？

我们可以先讲平方 function 抽成一个 N次 function

```typescript
// 传递一个 N 次值
const power = (n: number) => {
  return inputArray => {
   // 进行 N 次平方
   return inputArray.map(item => Math.pow(item, n));
  };
};

```

之后「平方」跟「立方」就可以透过这个power 的curry function 来产生：

```typescript
const square = power(2);
const cube = power(3);
console.log(`平方: ${square([1, 2, 3])}`); // 1, 4, 9
console.log(`立方: ${cube([1, 2, 3])}`); // 1, 8, 27

```

接下来「偶数平方和」和「偶数立方和」就可以各自运算啦：

```typescript
// 这里用之前的even 和 sum 
const sumEvenSquare = inputArray => {
  const evenData = even(inputArray);
  // 计算平方值
  const squareData = square(evenData);
  const sumResult = sum(squareData);
  return sumResult;
};
console.log(`偶數平方和: ${sumEvenSquare(data)}`);

const sumEvenCube = inputArray => {
  const evenData = even(inputArray);
  // 计算立方值
  const cubeData = cube(evenData);
  const sumResult = sum(cubeData);
  return sumResult;
};
console.log(`偶數立方和: ${sumEvenCube(data)}`);

```

上面的程式基本上都一样，只是把「平方」的function (第5 行) 换成了「立方」的function (第14 行)。所以我们也可以把这个function 视为是一种「可以被设定的参数」，写成新的 curry function

```typescript
// 由于多少次方和是可以抽象的，我们也可以 curry function
const sumEvenPower = powerFn => {
  return inputArray => {
    const evenData = even(inputArray);
    const powerData = powerFn(evenData);
    const sumResult = sum(powerData);
    return sumResult;
  };
};

// 建立偶數平方和的 function
const sumEvenSquare = sumEvenPower(square);
// 建立偶數立方和的 function
const sumEvenCube = sumEvenPower(cube);

console.log(`偶數平方和: ${sumEvenSquare(data)}`);
console.log(`偶數立方和: ${sumEvenCube(data)}`);

```

我们已经可以很好的运用curry function 了，但目前的程式中我们为了运算产生了很多变数，写起来还是有点烦，接着让我们运用compose 和pipe 两种技巧来解决这个问题。


### Compose

以前面例子的 sumEvenPower 来说，每次产生一个新的function 都宣告一个变数，稍微麻烦了一点，懒惰点的人可，懒惰点的人可能就直接写成一行

```typescript
const sumEvenPower = powerFn => {
  return inputArray => {
    // 也可以这样写，但是可读性就比较低了
    return sum(powerFn(even(inputArray)));
  };
};

```

这种写法就如同数学运算是一样，function 内在包个function。

不过这样一整行写起来可读性比较差一点，如果可以把要执行的function 都当作参数，再组合成新的运作function 就好了，这时候我们就可以写一个「组合函数(compose function)」：

```typescript
const compose = (...fns) => {
  return data => {
    let result = data;
    // 从最后一个 function 开始执行
    for (let i = fns.length - 1; i >= 0; --i) {
      result = fns[i](result);
    }
    return result;
  };
};

```

上面的例子中，我们宣告一个名称为compose function，这个参数就是我们想要传进去组合的所有function，由于最里面的function 会最先呼叫，因此要从传入的最后一个function 开始呼叫。

```typescript
const data = 1;
a(b(c(data))); // c先执行 依次是b、a

```

接着将所有要传入的function 传进去compose 内作为参数，就会自动帮我们组合出一个新的function

```typescript
const sumEvenPower = powerFn => { 
  // 执行顺序：even、powerFn、sum
  return compose(
    sum,
    powerFn,
    even
  );
};
const sumEvenSquare = sumEvenPower(square);
const sumEvenCube = sumEvenPower(cube);
 
console.log(`偶數平方和: ${sumEvenSquare(data)}`);
console.log(`偶數立方和: ${sumEvenCube(data)}`);

```

跟原本的写法比起来，透过compose function 来处理是不是清爽很多啊！而拜curry function 的lazy evaluation 特性以及我们设计的compose function 本身也是curry function 所赐，最终compose 出来的function 一样具有lazy evaluation 的特性喔！

另外，这种过程中都不用传入「资料」的做法(只有最后在真的带入资料)，也被称为Point Free的操作，算是在functional programming 中一种很常使用到的风格。


### Pipe

在compose function 的例子中，function 执行的顺序跟传入的顺序是刚好相反的，虽然这在数学上非常合理，但对于一般写程式的阅读习惯就不是这么一回事了，对一般程式阅读来说，传入的顺序最好跟执行顺序一样，可读性会好很多！

所以原来的compose function 可以把执行顺序调整成跟传入参数的顺序一样，我们称为「管道函数(pipe function)」：

```typescript
const pipe = (...fns) => {
  return data => {
    let result = data;
    // 原本 compose 的 for 循环从最后一个开始
    // pipe 内改为从第一个 function 开始執行
    for (let i = 0; i < fns.length; i++) {
      result = fns[i](result);
    }
    return result;
  };
};

```

之后的程式逻辑基本上都相同，只是组合function 的方法改成pipe，因此把顺序调整成符合一般阅读与执行习惯的顺序：

```typescript
const sumEvenPower = powerFn => {
  // 执行顺序：even、powerFn、sum
  return pipe(
    even,
    powerFn,
    sum,
   );
};
 
const sumEvenSquare = sumEvenPower(square);
const sumEvenCube = sumEvenPower(cube);
console.log(`偶數平方和: ${sumEvenSquare(data)}`);
console.log(`偶數立方和: ${sumEvenCube(data)}`);

```

对于一般习惯写程式的人来说，这种写法的可读性就再次增加


### Tap

最后我们来介绍tap！在使用functional programming 时，我们已经知道要尽量避开side effect（副作用） 了，也同时明白不可能完全没有side effect，因此要做的事情重点是把pure function 跟impure function 隔离，未来要查找问题也才更加知道如何处理，在functional programming 中，我们会写一个tap function，它其实没做什么事情，就是单纯不处理资料就直接回传，但允许我们再传入一个function 当参数来呼叫：

```typescript
// tap 內处理数据的 function 会执行传入的 function
// 但不会对传入的 data 做任何处理，就直接返回
// 目的是让我们在此处理一些 side effect
// 直接 data 方便传入的 function 继续处理资料
const tap = (fn) => {
  return (data) => {
    // 执行传入的 function
    fn(data);
    // 直接返回
    return data;
  }
}

```

之后所有的impure function 都必须传入tap function 来执行：

```typescript
const sumEvenPower = powerFn => {
  return pipe(
    even,
    // 用 tap 来隔离 side effect
    tap(data => console.log('even 后，目前资料为', data)),
    powerFn,
    tap(data => console.log('powerFn 后，目前资料为', data)),
    sum,
    tap(data => console.log('sum 后，目前资料为', data)),
  );
};
```

透过这种方式就可以妥善隔离pure function 和impure function 啦。

假设我们在impure function 做了有问题的操作，导致程式坏掉，在确认pure function 逻辑都正确的前提，我们就可以优先怀疑是tap function 内的程式有问题，找bug 也会更加轻松：

```typescript
// 一下在tab添加了 side effect
// 由于我们可以确定除了 tap 外都是 pure function
// 因此出问题时确定 pure function 逻辑没错，就可以先检查 tap 的逻辑
const sumEvenPowerBugVer = powerFn => {
  return pipe(
    even,
    tap(data =>{
      data.push(100);
    }),
    powerFn,
    sum,
  );
};

const sumEvenSquare2 = sumEvenPowerBugVer(square);
console.log(`偶數平方和: ${sumEvenSquare2(data)}`); // 結果和想的不一樣
```

tap function 一样是个curry function，搭配compose 和pipe 在真正传入资料开始运算之前，都不会呼叫tap 内的function，也代表在真正运算前不可能发生side effect 的情境，这也是确保程式稳定很重要的一个技巧！

当然还有一些隔离 side effect 的方法 比如 monad



## RxJS 中的FP 实际应用

接着我们来实际看看ReactiveX 中是如何应用前面提到的functional programming 技巧。了解这些背后的运作逻辑，对我们实际写程式会更有帮助，也会更加知道我们的程式到底在干嘛，以及工具帮我们做了些什么事情！



### Curry Function

以之前我们练习RxJS 时就学过的map operator 作为例子，会看到类似下面的程式码：

```typescript
export function map<T, R>(project: (value: T, index: number) => R, thisArg?: any): OperatorFunction<T, R> {
 return function mapOperation(source: Observable<T>): Observable<R> {
  ...
 };
}

```

可以看到map 本身就是个curry function，而 `project` 参数就是用来决定资料该如何转换的「**设定**」，内部的 `mapOperation` 是最终会被执行的方法，**带入的「资料」则是一个observable，同时也会回传一个observable**。

**所有的operators 基本上都是curry function，且输入输出都是observable**，因此我们可以直接带入参数后得到一个新的function 来处理observable 的结果，例如：

```typescript
import { Subject } from 'rxjs';
import { map } from 'rxjs/operators';

const source$ = new Subject<number>();

// 产生新的 function，参数必须为一个 observable
const double = map((data: number) => data * 2);
// 产生新的 function，参数必须为一个 observable
const plusOne = map((data: number) => data + 1);

// 组合一个新的 observable
const generateNextId$ = plusOne(double(source$));

generateNextId$.subscribe(data => console.log(data));

source$.next(1); // generateNextId$.subscribe 的結果是 3
source$.next(2); // generateNextId$.subscribe 的結果是 5
```

当然实际上我们几乎不可能会这样写，因为在RxJS 内我们也有内建的pipe 可以用！

### 使用Pipe 串起每个Operators

熟悉RxJS 的朋友相信都不会想用刚刚的写法来编写程式，实际上observable 会提供 `pipe` 方法，观念跟functional programming 的pipe 一样，让我们把operators (functions) 串起来：

```typescript
const source$ = new Subject<number>();
// 透过 pipe 将 source$ 换成新的 observable
const generateNextId$ = source$.pipe(
  map(data => data * 2),
  map(data => data + 1)
);
 
generateNextId$.subscribe(data => console.log(data));
source$.next(1); // generateNextId$.subscribe 的結果是 3
source$.next(2); // generateNextId$.subscribe 的結果是 5
```

由于每个operator 都是传入及回传一个observable 的curry function，搭配上pipe，回传自然就一样是个observable 啰。

如同functional programming 提到**higher order function**一样，把function 当作一等公民看待，在使用RxJS 时，我们也应该秉持**higher order observable**的精神，将Observable 也当作一等公民，传入传出都优先考量Observable，当慢慢习惯这样思考后，写起RxJS 就会越来越顺利啰。

有兴趣的话也可以看看[RxJS 的pipe 源码](https://github.com/ReactiveX/rxjs/blob/6.x/src/internal/util/pipe.ts#L24)，也是把每个function 组合起来的curry function！可见curry function 应用层面真的非常的广泛啊！

### 使用Tap 隔离Side Effect

最后我们来看side effect，不管是functional programming 还是ReactiveX，甚至是平常写程式时，都建议随时把**避免side effect**当作最高处理原则！而在ReactiveX 中，也有定义tap operator 来帮助我们处理side effect！

```typescript
const source$ = new Subject<number>();
const generateNextId$ = source$.pipe(
  map(data => data * 2),
  // 使用 tap 來隔离 side effect
  tap(data => console.log('目前数据', data)),
  map(data => data + 1),
  tap(data => console.log('目前数据', data))
);
 
generateNextId$.subscribe(data => console.log(data));
source$.next(1); // generateNextId$.subscribe 的结果是 3
source$.next(2); // generateNextId$.subscribe 的结果是 5
```
