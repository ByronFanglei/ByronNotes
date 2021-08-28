# TypeScript
1. JavaScript的超集，什么是超集，比如ES6就是ES5的超集
2. ts相对于js的优势：静态类型可以在编写代码发现错误，可以有更友好的提示
3. ts工具类方法

```typescript
// 初始化typescript 生成tsconfig.json文件，可以修改ts的默认配置
tsc --init
// 将ts编译为js
tsc index.ts
// 可以直接运行ts文件
ts-node index.ts
```

## TypeScript基础
### 静态类型
概念：就是说当一个变量初始话某个类型的值后，之后就不可以在接收其他类型的值，可以接收同类型的不同值

基础类型与对象类型
```typescript
// 基础类型 null undefined symbol boolean void number string
// void: 返回值为空
// never: 没有办法完全执行完
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
// 这里不用给函数注解是因为会自己推断出来
const getTotals = (str: string) => 123;
// 这里给函数注解是因为写法问题
const getTotal: (str: string) => number = (str) => 123;
// never: 这个函数永远不可能执行完
function errorEmitter(): never {
  throw new Error()
  // 或者
  while(true) {}
}
// 解构
function add({first, second}: {first: number, second: number}):number {
  return first + second
}

//  类型注解 type annotation 我们告诉TS变量是什么类型
//  类型推断 type inference  TS会尝试自动分析变量的类型
//  如果TS能够自动分析变量类型，我们就什么也不需要做了
//  如果TS无法分析变量类型，我们需要使用类型注解
```

### 数组与元组
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

### interface接口
只是作为开发过程中校验使用，并不会编译为js
```typescript
// interface与type的区别：interface只可以指定对象或者函数，type可以指定一个基础类型，能用interface就不用type
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
  // 接收参数                           返回类型
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

// 用接口表示数组 可索引类型，一般不会这么做
interface StringArray {
  [index: number]: String;
}

// 想要类继承接口需要使用implements语法
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

### 抽象类
一般来说当有多个类且具有共性时，我们需要做一个抽象类，抽象类只能继承不能被实例化，但是不同于接口，抽象类可以包含成员的实现细节，abstract用于定义抽象类和在抽象类内部定义抽象方法
```typescript
// 类 public private protected 访问类型
// public 允许我在类的内外被调用
// private 允许在类内被使用
// protected 允许在类内及继承的子类中使用
// constructor在new对象的时候就会触发

// get set 使用。可以对外保护私有属性
class Person {
  constructor(private _name: string) {}
  get name() {
    return this._name + ' qqq'
  }
  set name(name) {
    console.log('set', this._name); // byron
    this._name = name.split(' ')[0]
  }
}

const p = new Person('byron')
console.log(p.name); // byron qqq
p.name = 'byron qqq'
console.log(p.name); // byron qqq

// 单例模式 一个类只可以实例化一次
class Person {
  // 创建一个私有静态属性instance，类型为Person
  private static instance: Person
  private constructor(public name: string) {
    console.log(name ,'实例化了一次')
  }
  static getPerson () {
    if (!Person.instance) {
      Person.instance = new Person('hello')
    }
    return Person.instance
  }
}

const p1 = Person.getPerson();
const p2 = Person.getPerson();
console.log(p1 === p2); // true

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

### Ts安装问题
1. ts直接引用js会报错，需要安装对应的翻译文件也就是.d.ts为后缀的文件；
    ts -> .d.ts 翻译文件 -> js
2. 在使用tsc demo.ts时候是不会应用到tsconfig.json文件的，只有在使用tsc 时才会走tsconfig.json文件

### 一些小实战
1. 爬虫：首先将爬取页面的html先获取到

```shell
# 安装必要插件
# 安装superagent，可以在node环境下发送ajax
npm install superagent
# 安装cheerio，可以使用类似jq的语法获取标签的内容
npm install cheerio
# 安装concurrently 可以并行运行多个命令
npm install concurrently
```

2. 解析获取html
3. 将解析后的数据重构成自己需要的数据格式
4. 写入文件中

```javascript
// package.json的一些方法
"scripts": {
    "dev": "ts-node ./src/crowller.ts",
    // 添加-w 可以做到监听当前页面是否有变化
    "build": "tsc -w",
    // 可以监听项目文件是否发生改变并运行
    "start": "nodemon node ./build/crowller.js"
  },
// nodemon忽略文件变化
"nodemonConfig": {
    "ignore": ["data/*"]
  },
```

```json
{
  // 需要编译的内容
  "include": ["./index.ts"],
  // 不编译的内容
  "exclude": ["./demo.ts"],
  // 需要编译的文件
  "files": ["core.ts", "sys.ts", "types.ts",],
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */

    /* Basic Options */
    // 增量编译，只编译每次新增的内容
    "incremental": true,                         /* Enable incremental compilation */
    "target": "es5",                                /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', 'ES2021', or 'ESNEXT'. */
    "module": "commonjs",                           /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    // "lib": [],                                   /* Specify library files to be included in the compilation. */
    // 是否编译js文件
    "allowJs": true,                             /* Allow javascript files to be compiled. */
    // js语法检测
    "checkJs": true,                             /* Report errors in .js files. */
    // "jsx": "preserve",                           /* Specify JSX code generation: 'preserve', 'react-native', 'react', 'react-jsx' or 'react-jsxdev'. */
    // "declaration": true,                         /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                      /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // 生成sourceMap
    "sourceMap": true,                           /* Generates corresponding '.map' file. */
    // 所有输出的文件放到一个文件里
    "outFile": "./build/page.js",                             /* Concatenate and emit output to single file. */
    // 指定编译后存入的文件夹
    "outDir": "./build",                              /* Redirect output structure to the directory. */
    // 指定编译的文件夹
    "rootDir": "./src",                             /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                           /* Enable project compilation */
    // "tsBuildInfoFile": "./",                     /* Specify file to store incremental compilation information */
    // 移除注释
    "removeComments": true,                      /* Do not emit comments to output. */
    // "noEmit": true,                              /* Do not emit outputs. */
    // "importHelpers": true,                       /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,                  /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,                     /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                                 /* Enable all strict type-checking options. */
    // 如果一直值是any 那么需要指定
    "noImplicitAny": true,                       /* Raise error on expressions and declarations with an implied 'any' type. */
    // 强制检查值是否为null
    "strictNullChecks": true,                    /* Enable strict null checks. */
    // "strictFunctionTypes": true,                 /* Enable strict checking of function types. */
    // "strictBindCallApply": true,                 /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,        /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                      /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                        /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // 没有被使用的变量
    "noUnusedLocals": true,                      /* Report errors on unused locals. */
    // 没有使用的函数
    "noUnusedParameters": true,                  /* Report errors on unused parameters. */
    // "noImplicitReturns": true,                   /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,          /* Report errors for fallthrough cases in switch statement. */
    // "noUncheckedIndexedAccess": true,            /* Include 'undefined' in index signature results */
    // "noImplicitOverride": true,                  /* Ensure overriding members in derived classes are marked with an 'override' modifier. */
    // "noPropertyAccessFromIndexSignature": true,  /* Require undeclared properties from index signatures to use element accesses. */

    /* Module Resolution Options */
    // "moduleResolution": "node",                  /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // 当前项目的跟路径
    "baseUrl": "./",                             /* Base directory to resolve non-absolute module names. */
    // "paths": {},                                 /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                              /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                             /* List of folders to include type definitions from. */
    // "types": [],                                 /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,        /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                        /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,                    /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,                /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                            /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                               /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                     /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                       /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,              /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,               /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "skipLibCheck": true,                           /* Skip type checking of declaration files. */
    "forceConsistentCasingInFileNames": true        /* Disallow inconsistently-cased references to the same file. */
  }
}

```


## TypeScript语法
### 类型保护
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

// instanceof 语法做类型保护，这里需要定义class而不是interface，因为需要用instanceof来做判断
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

### 枚举类型

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

// 可以反查
console.log(GetReuslt(0)); // OFFLINE
```

### 函数泛型
泛指的类型，在使用函数的时候再进行类型的定义

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
  // 第二种写法 参数需要是T类型，返回也为T类型
const hello = <T>(params: T): T => {
  return params;
};
const func1: <T>(params: T) => T = hello;
```

### 类泛型

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

### 命名空间namespace

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

### parcel打包工具
1. parcel打包工具的使用：parcel类似于webpack打包工具但是不需要自己进行配置，可以自动识别加载的文件进行编译。

```shell
npm install parcel@next
```

### 如何定义全局变量或函数

```typescript
// 如何写一个.d.ts文件
// 参考用法
$(function () {
  console.log('123');
  $('body').html('<div>111111</div>');
  new $.fn.init();
});

// 定义全局变量  以jq为例子  关键字：declare 声明
declare var $: (praam: () => void) => void;

interface jQueryHtml {
  html: (html: string) => jQueryHtml;
}
// 定义全局函数  重载
declare function $(readyFunc: () => void): void;
declare function $(selector: string): jQueryHtml;

// 使用interface方法进行函数重载
interface jQuery {
  (readyFunc: () => void): void;
  (selector: string): jQueryHtml;
}
declare var $: jQuery;

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

### 类的装饰器
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

### 方法装饰器
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

### 访问器装饰器
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

### 属性装饰器
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

### 参数装饰器
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

