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
interface SayHi {
  // 函数接收一个string类型的参数同时返回一个string类型
  (word: string): string;
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












