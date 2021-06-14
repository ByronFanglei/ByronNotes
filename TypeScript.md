# TypeScript
1. JavaScript的超集，什么是超集，比如ES6就是ES5的超集
2. ts相对于js的优势：静态类型可以在编写代码发现错误，可以有更友好的提示

## TypeScript基础
1. 静态类型：什么意思，意思就是说当一个变量初始话某个类型的值后，之后就不可以在接收其他类型的值，可以接收同类型的不同值

基础类型与对象类型
```typescript
// 基础类型 null undefined symbol boolean void number string
const count: number = 1;
const teacher: string = 'byron';
let a: number | string = 1;
// 对象类型 {} class [] function
const tra: {
  name: string;
  age: number;
} = {
  name: 'byrons',
  age: 18,
};
// 数组
const number: number[] = [1, 2, 3];
// 类
class Person {}
const del: Person = new Person();
// 函数
const getTotals = (str: string) => 123;
const getTotal: (str: string) => number = () => 123;
// never: 这个函数永远不可能执行完
function errorEmitter(): never {
  throw new Error()
  // 或者
  while(true) {}
}

//  类型注解 type annotation 我们告诉TS变量是什么类型
//  类型推断 type inference  TS会尝试自动分析变量的类型
//  如果TS能够自动分析变量类型，我们就什么也不需要做了
//  如果TS无法分析变量类型，我们需要使用类型注解
```

2. 数组与元组
```typescript
// 数组
const arr: (number | string)[] = [1, 2, '3'];
const strArr: string[] = ['a', 'b', 'c'];
const undefinedArr: undefined[] = [undefined];
// obj类型
const objArr: { name: string; age: number }[] = [{ name: 'byron', age: 123 }];
// 类型别名 type alias
type User = { name: string; age: number };
const objArr1: User[] = [{ name: 'byron', age: 123 }];

// 元组 tuple，可以更准确的约束数组的类型，可以用来处理格式确定的文件
const teacherTuple: [string, string, number] = ['byron', 'hello', 123];
const teacherList: [string, string, number][] = [
  ['byron', 'male', 20],
  ['byrons', 'male', 24],
  ['jeny', 'female', 26],
];
```

3. interface接口，只是作为开发过程中校验使用，并不会编译为js
```typescript
// 接口
interface Person {
  // 只读key
  readonly rname: string;

  name: string;
  // 可以选择有或无
  age?: number;
  // 可以有一个字符串的key，value为任意值
  [proName: string]: any;
  //  设置一个返回值为string的函数
  say(): string;
}

// 接口继承
interface Teacher extends Person {
  teach(): string;
}

// 函数类型

interface SearchFunc {
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}

interface SayHi {
  // 函数名称为init，接收string类型的html和string类型的pathfile 返回一个string类型的值
  init: (html: string, pathfile: string) => string;
}

// 可索引类型，或者可以理解为数组
interface StringArray {
  [index: number]: String;
}

// 想要继承接口需要使用implements语法
class User implements Teacher {
  rname = '1';
  name = '1';
  say() {
    return '1';
  }
  teach() {
    return 'Teacher';
  }
}
```

4. 抽象类，抽象类一般不会被实例化，但是不同于接口，抽象类可以包含成员的实现细节，abstract用于定义抽象类和在抽象类内部定义抽象方法
```typescript
// 定义抽象类
abstract class Department {
  constructor(public name: string) {}

  printName(): void {
    console.log('Department name: ' + this.name);
  }

  abstract printMeeting(): void; // 必须在派生类中实现
}
// 继承抽象类
class AccountingDepartment extends Department {
  constructor() {
    super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
  }

  printMeeting(): void {
    console.log('The Accounting Department meets each Monday at 10am.');
  }

  generateReports(): void {
    console.log('Generating accounting reports...');
  }
}
// 进行实例化
let department: Department; // 允许创建一个对抽象类型的引用
department = new Department(); // 错误: 不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
department.printName();
department.printMeeting();
department.generateReports(); // 错误: 方法在声明的抽象类中不存在

```

5. ts直接引用js会报错，需要安装对应的翻译文件也就是.d.ts为后缀的文件；
    ts -> .d.ts 翻译文件 -> js


## TypeScript语法
1. 在使用tsc demo.ts时候是不会应用到tsconfig.json文件的，只有在使用tsc 时才会走tsconfig.json文件

2. 类型保护
```typescript
interface Brid {
  fly: string;
  say: () => {};
}
interface Dog {
  fly: string;
  sayHi: () => {};
}
// 类型断言方式进行类型保护
function Animate(animate: Brid | Dog) { // 联合类型，可以用Brid可以用Dog
  if (animate.fly) {
    (animate as Brid).say();
  } else {
    (animate as Dog).sayHi();
  }
}
// in 语法做类型保护
function Animat(animate: Brid | Dog) { // 联合类型
  if ('say' in animate) {
    animate.say();
  } else {
    animate.sayHi();
  }
}

// typeof 语法做类型保护
function add(one: string | number, two: string | number) { // 联合类型
  if (typeof one === 'string' || typeof two === 'string') {
    return `${one}+${two}`;
  }
  return one + two;
}

// instanceof 语法做类型保护
class NumberObj {
  // @ts-ignore
  count: number;
}

function addSecond(frist: Object | NumberObj, second: Object | NumberObj) { // 联合类型
  if (frist instanceof NumberObj && second instanceof NumberObj) {
    return frist.count + second.count;
  }
  return 0;
}
```

3. 枚举类型

```typescript
enum Status {
  OFFLINE,
  ONLINE = 11, // 可以设置初始值，下一个会对应加一
  LINELINE,
}

console.log(Status[0], Status[11]); // OFFLINE ONLINE

function GetReuslt(status: number) {
  if (status === Status.OFFLINE) {
    return 'offline';
  } else if (status === Status.ONLINE) {
    return 'online';
  } else if (status === Status.LINELINE) {
    return 'lineline';
  }
  return 'error';
}

console.log(GetReuslt(0)); // OFFLINE
```

4. 函数泛型：在使用函数的时候在进行类型的定义

```typescript
// 函数泛型 generic
function join<T>(first: T, second: T) {
  return `${first}${second}`;
}
join<number>(1, 1);

function map<T>(params: T[]) {
  return `${params}`;
}
map<string>(['123']);

function map1<T>(params: Array<T>) {
  return `${params}`;
}
map1<string>(['123']);
// 使用多个泛型
function join1<T, P>(first: T, second: P) {
  return `${first}${second}`;
}
join1<string, number>('1', 1);

// 如何使用泛型作为一个具体的类型注解
  // 第一种写法
const func: <T>(params: T) => T = <T>(params: T) => {
  return params;
};
  // 第二种写法
const hello = <T>(params: T) => {
  return params;
};
const func1: <T>(params: T) => T = hello;
```

5. 类泛型

```typescript
// 类泛型
// 基础类型
class DataManager<T> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index];
  }
}
const data = new DataManager<number>([1]);
data.getItem(0);

// 泛型继承
interface Item {
  name: string;
}
class DataManager1<T extends Item> {
  constructor(private data: T[]) {}
  getItem(index: number): string {
    return this.data[index].name;
  }
}
const data1 = new DataManager<Item>([
  {
    name: 'byron',
  },
]);

// 泛型指定联合继承
interface Test {
  name: string;
}
class DataManager2<T extends number | string> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index];
  }
}
const data2 = new DataManager<number>([1]);


// keyof 用法：简单来说可以遍历对象的类型，可以做到推断出正确的返回类型
interface Person {
  name: string;
  age: number;
  gender: string;
}

class Teacher {
  constructor(private info: Person) {}
  getInfo<T extends keyof Person>(key: T): Person[T] {
    return this.info[key];
  }
}
const t = new Teacher({
  name: 'byron',
  age: 23,
  gender: 'male',
});
const nn = t.getInfo('name');
console.log(nn); // byron
```

6. 命名空间namespace

```typescript
// components.ts
namespace Components {
  export namespace SubComponents {
    export class Test {}
  }

  export interface User {
    name: string;
  }

  export class Header {
    constructor() {
      const H = document.createElement('div');
      H.innerHTML = 'Header';
      document.body.appendChild(H);
    }
  }

  export class Content {
    constructor() {
      const C = document.createElement('div');
      C.innerHTML = 'Content';
      document.body.appendChild(C);
    }
  }

  export class Footer {
    constructor() {
      const F = document.createElement('div');
      F.innerHTML = 'Footer';
      document.body.appendChild(F);
    }
  }
}


// home.ts
// namespace：把一组相关变量封装到一起去，对外提供统一的封装接口
// 命名之间相互引用
///<reference path='./components.ts' />
namespace Home {
  export class Page {
    constructor() {
      new Components.Header();
      new Components.Content();
      new Components.Footer();
    }
  }
}

```

7. parcel打包工具的使用：parcel类似于webpack打包工具但是不需要自己进行配置，可以自动识别加载的文件进行编译。
8. 如何定义全局变量或函数

```typescript
// 参考用法
$(function () {
  console.log('123');
  $('body').html('<div>111111</div>');
  new $.fn.init();
});

// 定义全局变量  以jq为例子  关键字：declare 声明
// declare var $: (praam: () => void) => void;

interface jQueryHtml {
  html: (html: string) => jQueryHtml;
}
// 定义全局函数  重载
declare function $(readyFunc: () => void): void;
declare function $(selector: string): jQueryHtml;

// 使用interface方法进行函数重载
// interface jQuery {
//   (readyFunc: () => void): void;
//   (selector: string): jQueryHtml;
// }
// declare var $: jQuery;

// 如何对对象进行类型定义，以及对类进行类型定义，以及命名空间的嵌套
declare namespace $ {
  namespace fn {
    class init {}
  }
}

// 模块化写法
declare module 'jquery' {
  interface jQueryHtml {
    html: (html: string) => jQueryHtml;
  }
  function $(readyFunc: () => void): void;
  function $(selector: string): jQueryHtml;
  namespace $ {
    namespace fn {
      class init {}
    }
  }
  export default $;
}

```

9. 类的装饰器
```typescript
// 类的装饰器
// 装饰器本身就是一个函数
// 类装饰器接收的参数是构造函数
// 装饰器通过 @ 符号来使用，当类定义后调用装饰
// 可以一次调用多个装饰器，顺序从下往上
function TestDecorator1(flag: boolean) {
  if (flag) {
    return function Decorator(constructor: any) {
      constructor.prototype.getName = () => {
        console.log('byron');
      };
    };
  } else {
    return function Decorator(constructor: any) {};
  }
}
@TestDecorator1(false)
class Test {}
const test = new Test();
(test as any).getName();


// 进阶装饰器
function Decorator() {
  // 设置泛型，通过泛型进行构造实例化，
  return function <T extends new (...arg: any[]) => any>(constructor: T) {
    return class extends constructor {
      name = 'ma';
      getName() {
        return this.name;
      }
    };
  };
}

const Test = Decorator()(
  class {
    name: string;
    constructor(name: string) {
      this.name = name;
    }
  }
);
const t = new Test('byron');
console.log(t, t.getName());
```

10. 方法装饰器
```typescript
// 方法装饰器
// 普通方法，target对应的是类的 prototype
// 静态方法，target对应的是类的构造器

function getNameDecorator(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  console.log(
    target,
    `
    ${propertyKey}
  `,
    descriptor
  );
  // Test { getName: [Function] }
  // getName
  // { 这里可以想一想object之前学到的方法，都是类似的
  //   value: [Function],  当前的内容，现在也就是getName这个方法
  //   writable: true,   是否可以重写
  //   enumerable: true,  是否可以枚举
  //   configurable: true 是否可以删除
  // }
}
class Test {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  @getNameDecorator
  getName() {
    return this.name;
  }
}
const t = new Test('byron');
console.log(t.getName()); // byron
```

11. 访问器装饰器
```typescript
// 访问器装饰器
// TypeScript不允许同时装饰一个成员的get和set访问器。取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上。这是因为，在装饰器应用于一个属性描述符时，它联合了get和set访问器，而不是分开声明的
function setDecorator(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  // descriptor.writable = false;
}
class Test {
  private _name: string;
  constructor(name: string) {
    this._name = name;
  }
  get name() {
    return this._name;
  }
  @setDecorator
  set name(realname: string) {
    this._name = realname;
  }
}
const t = new Test('byron');
t.name = 'lee';
console.log(t.name);
```

12. 属性装饰器
```typescript
// 属性装饰器
function attributeDes(target: any, propertyKey: string): any {
  console.log(target, propertyKey); // Test {} name
  // 修改的并不是实例上的name， 而是原型上的name
  target[propertyKey] = 'maa';
  // 虽然属性装饰器上没有descriptor，但是我们可以自己搞，这样写完就不能对值进行修改了
  const descriptor: PropertyDescriptor = {
    writable: false,
  };
  return descriptor;
}
class Test {
  @attributeDes
  name = 'byron';
}

const t = new Test();
console.log(t.name); // byron
console.log((t as any).__proto__.name); // maa
t.name = '123'; // TypeError: Cannot assign to read only property 'name' of object

```

13. 参数装饰器
```typescript
// 参数装饰器
// 原型 方法名 参数下标
function ageDec(target: any, propertyKey: string, parameterIndex: number) {
  console.log(target, propertyKey, parameterIndex);
  // Test { getInfo: [Function] } getInfo 1
}
class Test {
  getInfo(name: string, @ageDec age: number) {
    console.log(name, age);
  }
}
const t = new Test();
t.getInfo('byron', 23);
```


