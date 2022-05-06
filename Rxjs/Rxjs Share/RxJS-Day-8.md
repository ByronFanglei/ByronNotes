# ReactiveX JavaScript（RxJS）- Day 8

## 从叠代器模式认识RxJS

### 关于叠代器

* 在物件导向程式设计里，叠代器模式是一种设计模式，是一种最简单也最常见的设计模式。它可以让使用者透过特定的介面巡访容器中的每一个元素而不用了解底层的实作。此外，也可以实作特定目的版本的叠代器

* 叠代器的重点在于「如何走访集合内的所有元素，并隐藏实作细节」


### 叠代器模式解决的问题

* 看一段代码

```typescript
// 日常遍历一个数组一般会这样
const data = ["a", "b", "c"];
for (let i = 0; i < data.length; i++) {
	console.log(data[i]);
}

// 然而，如果今天我的资料集合不是数组型态，而是一个树状结构呢？或尽管是数组型态，但必须有着不同的遍历规则呢？这时候当然就需要自己针对需要的资料结构或遍历规则而重新编码。
// 虽之而来的问题是，如何共享这些规则？这就是叠代器模式要处理的问题！
```

### 叠代器模式的角色定义

* 两个属性：

  * 叠代器 (Iterator)：用来存放集合的内容，除此之外更重要的是提供走访集合内容的底层实作，并公开出两个方法
    * next()：用来取得目前集合的下一个元素
    * hasNext()：用来判断是否还有下一个元素需要走访，当没有下一个元素需要走访时，代表已经完全走访过全部的元素


  * 聚合功能 (Aggregate)：用来产生叠代器实体的角色


### 使用JavaScript 实作叠代器模式

* 需求：「针对数组集合的走访，依照元素索引值(index)，先依序印出偶数索引值的内容，再印出奇数索引值得内容」

```typescript
const data = ["a", "b", "c", "d"];
const createEvenOddIterator = (data: any[]) => {
	let nextIndex = 0;

	return {
		hasNext: () => {
			return nextIndex < data?.length;
		},
		next: () => {
			let currentIndex = nextIndex;
			nextIndex += 2;
			if (nextIndex >= data?.length && nextIndex % 2 === 0) {
				nextIndex = 1;
			}
			return data[currentIndex];
		},
	};
};
const iterator = createEvenOddIterator(data);
while (iterator?.hasNext()) {
	console.log(iterator?.next());
}

```

* JavaScript 原生语法内建的叠代器 for of，也可以了解一下 generator


```typescript
const data = ["a", "b", "c", "d"];
for (let item of data) {
	console.log(item);
}

```


## ReactiveX 与叠代器模式

* 在ReactiveX 的实作中，我们也可以把叠代器模式的 next() 想像成观察者的next()，而叠代器模式中的 hasNext() 就像是观察者的complete()，差异只在处理的是集合还是串流而已



