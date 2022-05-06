# ReactiveX JavaScript（RxJS）- Day 11

## 认识弹珠图

## 关于弹珠图

由于ReactiveX 具有stream 的观念，加上有大量的operators 来帮助我们改变stream 的资料流向，因此如何表达资料的流向就非常重要！在ReactiveX 中，我们会使用弹珠图来表达资料流向，因此能够绘制及阅读弹珠图就变成学习ReactiveX 必备的技术！

### 如何绘制弹珠图

接下来我们将实际绘制一个简单的弹珠图，除了阅读文章之外，也欢迎实际拿纸出来画画看喔！

#### 画出资料流

首先，我们需要绘制一条横线，代表时间轴，时间轴的左边代表「过去」，右边带表「未来」：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617vg5t9Lh3Hr.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617vg5t9Lh3Hr.jpg)

在时间轴上，会有各式各样的「事件」( `next()`)发生，这种事件在图上都会用一颗「弹珠」来表示，并于弹珠内写上事件的「值」：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617e3qKGiH8i8.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617e3qKGiH8i8.jpg)

如果这个资料流结束( `complete()`) 了，则会在结束的时候标记一个垂直符号( `|`)：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617etgufMWtX1.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617etgufMWtX1.jpg)

如果这个资料流有发生错误( `error()`)，则标记一个错误符号( `X`)：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617KZHG9GIfJ1.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617KZHG9GIfJ1.jpg)

### 加入Operator

除了建立类型外的operators (之后我们再来说明operators 有哪些类型) 都是将一个observable (也就是stream) 换成另一个observable，在弹珠图上，我们会在来源资料流下面加上要使用operator 及使用方法，例如：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617GkyOG55es1.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617GkyOG55es1.jpg)

上述例子我们用了 `map` 这个operator，并将每个流入operator 的资料「加一」，因此会产生一条新的observable，在弹珠图上就可以画出一条新的时间轴：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617zLu2HZIHzN.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617zLu2HZIHzN.jpg)

如果有多个operators 呢？就持续补上「使用的operator」及「新的observable」就好：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617XFL5CpmlaZ.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617XFL5CpmlaZ.jpg)

如果相对好理解，当然也可以简化每次产生新的observable 的行为，画出最终结果就好：

![https://ithelp.ithome.com.tw/upload/images/20200926/20020617szq4C3fm1A.jpg](https://ithelp.ithome.com.tw/upload/images/20200926/20020617szq4C3fm1A.jpg)

很简单吧！透过弹珠图，可以把复杂的程式码简化成好阅读的资料流向图，在沟通和理解也会变得更加简单喔！



### 文字版弹珠图

在白板上画出弹珠图是与人沟通资料流向的最佳管道，但有些时候我们无法直接与人面对面画出弹珠图来沟通，尤其是远端工作盛行的现在，或是在撰写文章/文件时，如果还要用电脑软体画出弹珠图，反而会花费更多时间，所以还有一种版本的弹珠图，是单纯用电脑文字来表示的。文字版弹珠图的组成要素如下：

- 使用 `-` 代表一个时间点单位不拘，方便显示就好，以下是一条时间轴

```
-----------------------------------
```

- 当有事件发生时，直接在该时间上把 `-` 取代成发生的资料

```
-----1-----2-----3-----4-----------
```

- 另外，如果有资料是在同一个时间点发生，则可以用小括号包起来

```
-----(12)--3-----4-----------------
```

这里的 `(12)` 代表1 和2 两个值是在同一个时间点发生的

- 有时候为了对齐方便，会直接用空白 ``来当作没任何事情，也没有任何时间概念(纯粹对齐用)

```
-----(12)--3-----4-------
----- a  --b-----c-------
     ^ 這邊 (12) 和 a 代表同樣時間點
```

在资料是同步的时候，不会加上单位时间的符号`-`，也可以使用空白符号方便内容对齐，阅读上也会更方便

```
(1      2      3      4      |)
(a      b      c      d      |)
```

上面两个资料流都是同步发生的，但为了让画面更清楚，使用空白符号让资料不要挤在一起。

- 当资料流结束，加上`|`

```
-----1-----2-----3-----4-----------|
```

- 如果事件发生完立刻结束呢？一样用小括号把资料含结束符号包起来就好

```
-----1-----2-----3-----(4|)
```

- 当发生错误时，加上`#`

```
-----1-----2-----3-----4------#----
```

- 有operator 时，一样在原来时间轴下方写下operator 的使用

```
-----1-----2-----3-----4-----------|
map(x => x + 1)
```

- 然后画出新的时间轴(observable)

```
-----1-----2-----3-----4-----------|
map(x => x + 1)
-----2-----3-----4-----5-----------|
```

- 有多个operators 时，一样持续加入「使用的operator」及「新的observable」

```
-----1-----2-----3-----4-----------|
map(x => x + 1)
-----2-----3-----4-----5-----------|
filter(x => x % 2 === 0)
-----2-----------4-----------------|
```

- 或简单加上最后结果

```
-----1-----2-----3-----4-----------|
map(x => x + 1)
filter(x => x % 2 === 0)
-----2-----------4-----------------|
```

很简单吧！在使用文字版弹珠图时，建议一律使用等宽字(如Consolas [等宽字体] 等)，在排版上会方便许多！使用markdown 时，程式码区块基本上都是等宽字，所以处理起来真的很方便哩。

这种文字弹珠图的表示方式会是未来我们介绍operators 时主要的说明方式，比较复杂的情境才会考虑画图处理；而在之后介绍RxJS 测试时也会用到(测试时会有更多符号)。