# ReactiveX JavaScript（RxJS）- Day 9

## 函数式编程

### 关于函式语言程式设计

* 概念：函式语言程式设计（英语：functional programming）或称函式程式设计、泛函编程，是一种编程范式，它将电脑运算视为函式运算，并且避免使用程式状态以及易变物件

* 比起指令式编程，函数式编程更加强调程式执行的结果而非执行的过程，倡导利用若干简单的执行单元让计算结果不断渐进，逐层推导复杂的运算，而不是设计一个复杂的执行过程。

* 在函式语言程式设计中，函式是第一类物件，意思是说一个函式，既可以作为其它函式的参数（输入值），也可以从函式中返回（输入值），被修改或者被分配给一个变数


### 状态相依和物件改变

在开发时，完全避免**外部状态相依**和**物件的改变**是一件非常困难的事情，以下举一个不太好的程式码，当我们要计算一个数组中的偶数平方合时，可能会写出这样的程式码：

```typescript
const data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let sum = {
  value: 0
};

// 计算数组偶数平方合
const sumEvenSquare = () => {
  for (let i = 0; i < data.length; ++i) {
    const value = data[i];
    if (value % 2 === 0) {
      sum.value += value * value;
    }
  }
};

sumEvenSquare();
console.log(sum.value); // 220

```

sum和 data 我们会称为是一个外部的「状态」，这个状态可能永远都不会被改变，却也可能在某个时间点忽然被改变了。

而 sumEvenSquare 是一个用来计算我们所需结果的function，它直接相依了 data 和 sum 两个外部的状态，同时在运算过程中，它改变了 sum 这个外部物件，这代表外部的状态也跟个被改变了！

这其实是非常可怕的一件事情，我们在计算过程中不断的在改变 sum 资料，如果今天 sumEvenSquare 执行两次，我们是否会得到一样的结果呢？

```typescript
sumEvenSquare();
console.log(sum.value); // 220
sumEvenSquare();
console.log(sum.value); // 440

```

答案显而易见的是「不会」，因为我们在运算第一次后改变了 sum 的资讯，却没有重设 sum 的值，因此第二次运算的时候会继续改变 sum 的内容。

再假设一下，复杂的情境下，我们可能会让 sumEvenSquare 独立一个执行去计算，以增进整体效能，此时若是呼叫两次 sumEvenSquare 会变成怎样呢？当 sumEvenSquare 是个更复杂、耗时更长的运算时，我们不小心改变了 sum 或data，会发生什么事情呢？

答案是：我也不知道！

因为运作过程中可能被改变的情境太多了！在程式设计中不能掌控程式运作过程中资料被变动的情况，也就代表着在越容易产生bug，同时也越难查出bug 发生的情况，也就越难修正、越容易改A 坏B。

这种情况我们会成为**「非执行绪安全(Non-thread Safe)」**的计算，是我们应该极力去避免的！那么怎样达到**「执行绪安全(Thread Safe)」**呢？


### 避免外部状态相依和改变

最简单的方式是将这些状态从外部拉到内部，让他的作用域只能发生在内部：

```typescript
const sumEvenSquare = (dataArray: number[]) => {
  // 把计算值放在内部
  let innerSum = {
    value: 0
  };

  // 计算总合
  for (let i = 0; i < dataArray.length; i++) {
    const value = dataArray[i];
    if (value % 2 === 0) {
      innerSum.value += value * value;
    }
  }

  // return 内部值
  return innerSum;
}

// 每次传入数组，都要先复制一份新的，避免数据被更改
const sum1 = sumEvenSquare([...data]);
console.log(sum1.value); // 220
const sum2 = sumEvenSquare([...data]);
console.log(sum2.value); // 220

```

透过这种方式，我们就能避免在运算时发生「外部状态改变」的情境，由于也不会去更改到外部状态，因此不用担心重复呼叫造成外部状态资料无法掌握的问题！

**而这种相依或改变外部状态资料的行为，我们通常又称为side effect (副作用)。**

Side effect 的范围非常广范，只要是与外部状态有关的，都算是side effect，例如：

全域变数
DOM 物件操作
console.log
HTTP 请求呼叫
...



避免side effect，把所有资料都变成参数或内部状态的function，又称为**「pure function (纯函数)」**。

另外，我们透过 [...data] 的方式传入一个新的阵列，可以避免原来的 data 阵列被改变，除此之外，我们也可以用一些不会改变身列本身内容的操作来操作阵列内容，例如新增一个新元素，比起使用data.push(100)，使用 const data2 = [...data, 100] 这种写法可以让我们不要改到 data 本身，这种操作我们成为**「immutable (不可变的)」**。

不管要不要朝 functional programming 前进，在设计程式时，都建议应该要尽量将 function 设计成 pure function ，同时在操作资料时，也要尽量朝向 immutable 的方式去设计，以避免难以掌握状态的情境发生！

Pure function 还有一个很重要的特性，就是不管执行几次，一样的输入就一定会有一样的输出，不会因为任何原因在不同时间产生不同的输出，由于pure function 基本上就不会相依任何外部状态，加上immutable 的操作，因此这可以说是必然的结果；如果无法达成这些条件，就不可称它为pure function，而会称为「impure function (不纯的函数)」。


### 与Side Effect 保持正确关系

当然在实际开发中，要完全避免side effect 是不可能的事情，以网页开发来说，画面上的每个元素对目前设计的 sumEvenSquare 都是外部状态，但我们不可能不透过这些元素来进行互动，例如从某个输入框得到画面的资料，再把结果放到画面上等等，这些都是side effect，但是核心的处理资料逻辑，我们就应该以pure function 来作为设计的中心。

方法很简单：把side effect 的操作整理成要传入pure function 的参数(input)，再将执行结果(output) 拿去做其他的side effect。

```typescript
// 计算 "加 1" 的 pure function
const plusOne = (value: number) => value + 1;

document.querySelector('#plusOneBtn').addEventListener('click', () => {  
  // 整理值给 pure function 的 input，这里是 side effect
  const input = +(document.querySelector('#val') as HTMLInputElement).value;

  // 计算出 output，因为是 pure function
  const output = plusOne(input);

  // 计算结果进行互动，这里也是 side effect
  document.querySelector('#result').innerHTML = output.toString();
});

```

这么一来，重要的运算逻辑又不会与外部状态相依，也更加容易找出问题！


### 函数是一等公民

以前面的 sumEvenSquare 例子来说，假设我们希望可以更有弹性，除了「偶数」「平方」和以外，还能给予更多不同的运算方式时，我们可以把偶数和平方的计算方式拆出来，单纯计算数组的和，但数组本身每个值的运算(如过滤出偶数和转换成平方)等变成可以被外部呼叫，大概会写成这样：

```typescript
// 把数组中值的转换成 processFn
const sum = (processFn: (input: number) => number, inputArray: number[]) => {
  let result = 0;
  for(let i = 0; i < inputArray.length; ++i) {
    // 直接用传入的 processFn 即可
    result += processFn(inputArray[i]);
  }
  return result;
}

```

上述程式我们把原来计算偶数平方的逻辑拿掉，变成一个可以传入的 function，当要计算总和时，再针对每个值去呼叫这个 function 进行运算，等于把每个值运算的细节拆出去了。而因为 function 是一等公民，所以可以把 function 当作参数传入另一个 function 中。

除此之外，由于 function 是一等公民，因此一个 function 也能回传另一个 function，以上述例子来说，处理方法和资料是拆开的，我们可以先得到一个运算结果的 function

```typescript
// 把数组中值的转换成 processFn
const sum = (processFn: (input: number) => number) => {
  // 直接返回一个function
  return (inputArray: number[]) => {
    let result = 0;
    for(let i = 0; i < inputArray.length; ++i) {
      // 直接用传入的 processFn 即可
      result += processFn(inputArray[i]);
    }
    return result;
  }
}

```

另外要计算偶数平方时，可以再写一段function 来运算每个值：

```typescript
// 计算平方，奇数返回0
const evenSquare = (item: number) => {
  return item % 2 === 0 ? item * item : 0;
};

```

此时计算偶数平均和的函数会变成一个组合出来的结果

```typescript
// 先得到一個 function
const sumEvenSquare = sum(evenSquare);

```

最后再把资料带入这个 sumEvenSquare 内：

```typescript
// 再以剛才得到的 function 實際進行運算
const myResult1 = sumEvenSquare(data);
console.log(myResult1);

```

透过这种设计方式，我们可以将许多细节都抽出来，只留下一个比较抽象的运作过程，而被抽出来的部分，因为只需要关注在它自己要做的事情就好，所以程式码会更加简短，加上pure function 的设计，整体的可维护性就会更加好啦！

这种function 被视为一等公民写法，必须要程式语言本身支援，现在许多程式语言也都支援这种写法了，而这种特色也称为「higher order function (高阶函数)」。


### 命令式(Imperative) vs 宣告式(Declarative)

* 命令式Imperative：强调的是执行过程，通常会暴露非常多细节，比较具象

* 宣告式Declarative：强调的是执行结果，在思考过程中会隐藏细节，比较抽象

具象的好处是我们可以比较容易看到程式运作的过程和所有细节；而抽象的最大的好处正好相反，是隐藏细节；可以想想看当我们想要快速理解一本书中介绍些什么东西，最快的方式是什么？相信大多数人都会接受比起直接从正文一页一页看起，先阅读目录是比较好的方法，透过目录可以让我们对于一本书整体想传达的知识有个基本认知，甚至可以协助我们快速判断需不需要继续阅读下去。

在程式开发也是一样，如果我们用「目录」的概念(也就是declarative 宣告式的概念) 去写程式，就会变成如下：

```typescript
const data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// 以下步驟呼叫的 even、square、sum 
// 过滤偶数
const evenData = even(data);
// 计算平方值
const evenSquareData = square(evenData);
// 加法計算
const sumResult = sum(evenSquareData);

console.log(sumResult);

// 哈哈哈 也可以这么写
console.log(sum(square(even(data))));

```

比较一下前面imperative 式的写法：

```typescript
for (let i = 0; i < data.length; ++i) {
  const value = data[i];
  if (value % 2 === 0) {
    sum.value += value * value;
  }
}

```

虽然程式码行数变多了，但整体阅读起来是不是觉得declarative 式的写法好懂非常多啊！至少我们不用去思考 for 和 if 是做些什么的，只要从function 名称做基本推断就可以了

可以看到每个function 都是一个简单的「运算单元」，都有单一的input 和output，最后资料「渐进式」的变成了我们想要的结果。

当发生bug 时，也只需要针对每个function 的结果去判断问题是否在该function 里面就好，当习惯这种写法后，真的会觉得程式的编写跟阅读起来都会非常过瘾啊！！

前面所有隐藏的实作，我们都是用 for 和 if 来处理资料，如果是熟悉JavaScript 阵列处理相关API 的人，应该知道filter、map和 reduce 这三个API，其实也是帮助我们把细节处理变得更加declarative，接下来我们把实作细节也从 for 和 if 中解放：

```typescript
// 把原來细节变得更加 declarative
const even = (inputArray: number[]) => {
  return inputArray.filter(item => item % 2 === 0);
};

const square = (inputArray: number[]) => {
  return inputArray.map(item => item * item);
};

const sum = (inputArray: number[]) => {
  return inputArray.reduce((previous, current) => previous + current, 0);
};

```

有没有发现，原来我们平常就默默的在使用functional programming 的一些观念实作程式啦！

其实，如果你觉得特地把even、square和 sum 抽出来很啰唆，也可以直接写成：

```typescript
const data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const result = data
  .filter(item => item % 2 === 0)
  .map(item => item * item)
  .reduce((previous, current) => previous + current, 0);
console.log(result);

```

依然是很宣告式的写法，对于情境不复杂的情况来说，也很ok 的！filter、map和 reduce 本身设计已经帮我们隐藏了走访每个数组值的细节，将处理数组值的实作方式交给我们自己决定，而这些实作要开放到什么程度，或是否要隐藏起来，就看实际设计时是否觉得已经够好阅读了，或是实作是否太复杂来决定啰！

最后，我们把上述写法再复杂化一下，假设filter、map和 reduce 内目前的逻辑很复杂，我们可以把这些逻辑再次抽出来：

```typescript
const byEven = item => item % 2 === 0; // 假設它很複雜
const toSquare = item => item * item; // 假設它很複雜
const sumTwoValues = (value1, value2) => value1 + value2; // 假設它很複雜

```

最后程式就只剩下：

```typescript
const result = data
  .filter(byEven)
  .map(toSquare)
  .reduce(sumTwoValues, 0);
console.log(result);

```

是不是就像在阅读一篇文章一样，而非在阅读复杂逻辑的程式码啦！！

>
> 其实不管复杂或简单的逻辑，都建议将各种**「意图」抽成一个一个小function**，最后再一口气组合起来，爽度绝对是爆表啊！！





* 避免side effect（副作用），把所有资料都变成参数或内部状态的function，又称为**「pure function (纯函数)」**
* Pure function 还有一个很重要的特性，就是不管执行几次，一样的输入就一定会有一样的输出，不会因为任何原因在不同时间产生不同的输出，由于pure function 基本上就不会相依任何外部状态，加上immutable 的操作，因此这可以说是必然的结果；如果无法达成这些条件，就不可称它为pure function，而会称为**「impure function (不纯的函数)」**。