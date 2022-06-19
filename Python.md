# Python

## 安装

```shell
brew install python3

# 测试
python3
  Python 3.9.12 (main, Mar 26 2022, 15:51:15)
  [Clang 13.1.6 (clang-1316.0.21.2)] on darwin
  Type "help", "copyright", "credits" or "license" for more information.
	>>>

# 退出
exit()
```


## python基础

0. 一些常用的方法

```python
# int


# str
1. '123.456'.index('.')  字符串获取制定字符的位置
2. ''.join(["a", "b", "c"])  str类型转换为字符串
3. strip() 方法用于移除字符串头尾指定的字符（默认为空格或换行符）或字符序列



# list
1. '123.456'.split('.'). 字符串转换为数组

# dict



```



1. 基本命令

```python
# Python的整数没有大小限制, Python的浮点数也没有大小限制，但是超出一定范围就直接表示为inf（无限大）
# 当python输出很大的数字时，Python允许在数字中间以_分隔
print(10_000_000_000)
print(10000000000)

# 转义字符，跟其他语言转义大致相同，这里说一下python不转义的方法 r''
print('q \t q') # q        q

print(r'q \t q') # q \t q

# 如果字符串内部有很多换行，用\n写在一行里不好阅读，为了简化，Python允许用'''...'''的格式表示多行内容
print('''
    第一行
    第二行
    第三行
''')
#    第一行
#    第二行
#    第三行

print('''hello,\n
world''')
# hello,\n
# world

print(r'''hello,\n
world''')
# hello,

# world


# 布尔值 True and False
# and运算是与运算，只有所有都为True，and运算结果才是True
# or运算是或运算，只要其中有一个为True，or运算结果就是True
# not运算是非运算，它是一个单目运算符，把True变成False，False变成True

# 空值 None，None不能理解为0，因为0是有意义的，而None是一个特殊的空值

a = 'ABC'
b = a
a = 'XYZ'
print(a, b)
# XYZ ABC

# 除法，/除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数
print(9 / 3) # 3.0
# 还有一种除法是//，称为地板除，两个整数的除法仍然是整数
print(9 // 3) # 3
print(10 // 3) # 3


# 输入
name = input()

# 输出
print(name)

```


2. 字符串编码

```python
print(r'/]\\dasd\fdsf\n/n')  # r'' print 使用 r'' 内的字符串不会自动转义
print(r'''hello,\n
world''')  # ''' ... ''' 语法可以展示多行，也可以跟 r'' 一起用

# ord()函数获取字符的整数表示
ord('房')
25151

# chr()函数把编码转换为对应的字符
chr(25151)
'房'

# encode()方法可以编码为指定的bytes
'ABC'.encode('ascii')
b'ABC'
'中文'.encode('utf-8') # 如果 str 有中文就不能用 ascii
b'\xe4\xb8\xad\xe6\x96\x87'

# decode()方法可以将bytes转为str
b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore') # 如果要忽略错误，添加errors='ignore'
'中'

# 计算str包含多少个字符，可以用len()函数，len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数
len('ABC') # 3
len(b'ABC') # 3
len('中文') # 2
len(b'\xe4\xb8\xad\xe6\x96\x87') # 6

# tip：由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码

# 格式化，在字符串内部，%s表示用字符串替换，%d表示用整数替换，%f表示用浮点数，%x表示用十六进制，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略
print('Hello, %s' % 'world') # 'Hello, world'
print('Hi, %s, you have $%d.' % ('Michael', 1000000)) # 'Hi, Michael, you have $1000000.'

# 如果 % 是一个普通字符这样处理
print('growth rate: %d %%' % 7) # growth rate: 7 %

# format 格式化方法，比较麻烦
print('Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)) # 'Hello, 小明, 成绩提升了 17.1%'

# f-string 格式化处理方法
r = 2.5
s = 3.14 * r ** 2
print(f'The area of a circle with radius {r} is {s:.2f}')
# The area of a circle with radius 2.5 is 19.62

```

3. list，tuple

```python
# list 类似js数组
classname = [1,2,3,4,5]
print(len(classname)) # 5
print(classname[0], classname[-1], classname[len(classname) - 1]) # 1 5 5

# 尾部添加元素
classname.append(111) # [1, 2, 3, 4, 5, 111]
# 指定位置添加元素
classmates.insert(1, 'Jack') # ['Jack', 1, 2, 3, 4, 5, 111]
# 删除尾部元素 / 删除指定位置元素，返回删除的元素
classname.pop()
classname.pop(1)

# tuple 元组 tuple和list非常类似，但是tuple一旦初始化就不能修改, 不可变保证了更安全，如果可能，能用tuple代替list就尽量用tuple
classmates = ('Michael', 'Bob', 'Tracy')
# 当定一个只有一个元素的元祖 需要这样设置
classmates = ('Michael',)

list(range(5)) # range 生成一个指定整数序列
[0, 1, 2, 3, 4]
```

4. 条件判断

```python
# if语句执行有个特点，它是从上往下判断，如果在某个判断上是True，把该判断对应的语句执行后，就忽略掉剩下的elif和else
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```


5. 循环

```python
# for in
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
    
# while    
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```


6. dict，set

* dict和list比较，dict有以下几个特点：

	1. 查找和插入的速度极快，不会随着key的增加而变慢；
	2. 需要占用大量的内存，内存浪费多。

* 而list相反：

	1. 查找和插入的时间随着元素的增加而增加；
	2. 占用空间小，浪费内存很少。

```python
# dict 类似于其他语言的map
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
print(d['Michael']) # 95

# 添加
d['Adam'] = 67

# 判断 key 是否存在
print('Bob' in d) # True
print('Boba' in d) # False


# 通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value
print(d.get('Michael')) # 95 
print(d.get('Thomas')) # None 
print(d.get('Thomas', '么有')) # '么有'


# 删除某个值
d = {'Michael': 95, 'Tracy': 85, 'Bob': 75}
print(d.pop('Bob')) # 75
print(d) # {'Michael': 95, 'Tracy': 85}


# set类似于 js 的Set，重复元素会被自动过滤
s = set([1, 2, 3])	# 写用的 []
print(s) # {1, 2, 3} 查看时 {}

# 添加元素
a.add(111)

# 删除元素
a.remove(111)

s1 = set([1, 2, 3])
s2 = set([2, 3, 4])
# 两个元素的交集
print(s1 & s2) # {2, 3}
# 两个元素的并集
print(s1 | s2) # {1, 2, 3, 4}
```

* dict与list比较

dict：dict是用空间来换取时间的一种方法
*   查找和插入的速度极快，不会随着key的增加而变慢；
*   需要占用大量的内存，内存浪费多。

list：
*   查找和插入的时间随着元素的增加而增加；
*   占用空间小，浪费内存很少。


## 函数部分

1. [常用函数](https://docs.python.org/3/library/functions.html)

```python
# abs 绝对值
print(abs(100)) # 100
print(abs(-20)) # 20
print(abs(12.34)) # 12.34

# 最大最小
print(max(2, 3, 1, -5)) # 3
print(min(2, 3, 1, -5)) # -5


# 数据类型转换
print(int(1), int('1'), int(True), float('12.34'), str(1.23), str(100), bool(1), bool(''))
# 1 1 1 12.34 1.23 100 True False
```


2. 定义函数

```python
# def 定义函数
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
        
# 空函数， pass做占位符
def nop():
    pass

# 返回多个参数
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

r = move(100, 100, 60, math.pi / 6)
print(r) # (151.96152422706632, 70.0), 返回的是一个 tuple

# 判断当前参数类型
print(isinstance(1, int)) # True
print(isinstance(1, (int, float))) # True

```

3. 函数参数

```python
# 函数的默认参数与js用法基本一致
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)

enroll('Bob', 'M', 7) # 按顺序写参数值
enroll('Adam', 'M', city='Tianjin') # 可以直接写参数名定义某个参数

# 参数前加 *会使参数numbers接收到的是一个tuple，可以立即成 js 的arguments
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
    
calc(1, 2, 3, 4, 5)
# 打印list 或者 tuple 加* 可以理解为 js 的解构
nums = [1, 2, 3, 4, 5]
print(*nums, nums) # 1 2 3 4 5 [1, 2, 3, 4, 5]


# 关键字参数
# **extra表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

extra = {'city': 'Beijing', 'job': 'Engineer'}
person('byron', 18, **extra)
# name: byron age: 18 other: {'city': 'Beijing', 'job': 'Engineer'}


# 以上操作是可以传递任意命名的关键字参数，如果需要传递指定命名的参数需要在* 号后面添加参数
def person(name, age, *, city, job):
    print(name, age, city, job)
    
person('Jack', 24, city='Beijing', job='Engineer')

# 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
def person(name, age, *args, city, job):
    print(name, age, args, city, job)


def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw, isinstance(args, tuple), isinstance(kw, dict))

f1(1, 2, 3, 111, 222, 333, aa=1, bb=2)
# a = 1 b = 2 c = 3 args = (111, 222, 333) kw = {'aa': 1, 'bb': 2} True True


# 目前认为就是入参中一个 * 可以代表一个或者多个常量的tuple， 两个 * 可以代表一个或多个key value的形式，最后展示出来的就是dict类型
# 所以反过来想 func(*args, **kw) 这个就是一个万能函数。。。牛皮

# *args是可变参数，args接收的是一个tuple
# **kw是关键字参数，kw接收的是一个dict

```


4. 递归函数

```python
# 简单的递归函数
def fact(n, s=1):
    if n > 1:
        s = s * n
        return fact(n - 1, s)
    else:
        return s
        
# 调用递归会有栈溢出的问题，解决递归调用栈溢出的方法是通过 尾递归 优化，事实上尾递归和循环的效果是一样的，所以，把循环看成是一种特殊的尾递归函数也是可以的。尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

# 没有使用尾递归
def fact(n):
    if n == 1:
        return 1
    return n * fact(n - 1)

print(fact(5))


# 使用尾递归
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)

print(fact(5))

# 这里怎么感觉使用了尾递归就是把栈溢出的可能嫁接到另一个函数了
```


## 函数部分

1. 高级特性

```python
# 切片 [0:3] 意思就是 从0开始截取3个，当然如果是0开始的话可以不用写0 直接[:3], 当然也支持负数，倒数第一个元素的索引是-1
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
print(L[1:3], L[-2:], L[-2:-1])
# ['Sarah', 'Tracy'] ['Bob', 'Jack'] ['Bob']


# [0::5] 表示 从0开始每5个取一次，如果是0开始的话，这个0可以不写
# 甚至可以这样 L[:]，直接复制当前的 list
L = list(range(20))
print(L[:10:2]) # [0, 2, 4, 6, 8] 表示前十个数字，每两个取一个数
print(L[::5]) # [0, 5, 10, 15] 表示每5个取一个数字
print(L[10::5]) # [10, 15] 表示从10开始每5个取一个数字
print(L[:]) # 表示复制当前list
print(L[::-1]) # ['Jack', 'Bob', 'Tracy', 'Sarah', 'Michael'] 反转当前字符串，很有用，优雅 

# tuple也可以使用切片操作，只不过操作后的还是tuple

# 字符串也可以这么搞
print('ABCDEFG'[:3]) # 'ABC'
print('ABCDEFG'[::2]) # 'ACEG'

# for 循环 dict
d = {'a': 1, 'b': 2, 'c': 3}
# 循环key
for key in d:
    print(key)
# 循环value
for value in d.values():
    print(value)
# 循环key value
for k, v in d.items():
    print(k, v)

# 如果要打印list的下标可以使用enumerate包装一下
for i, value in enumerate(['A', 'B', 'C']):
     print(i, value)

# list的下标循环
for k, v in enumerate([1, 2, 3]):
    print(k, v)
# 0 1
# 1 2
# 2 3

# fro循环引入两个变量 (整挺好)
for x, y in [(1, 1), (2, 4), (3, 9)]:
    print(x, y)
# 1 1
# 2 4
# 3 9

# range 要生成list [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]可以用list(range(1, 11))
# list(range(1, 10, 3), 表示生成一个从 1 开始 10 结束的list，每个 3 个取一个值
[1, 4, 7]

# 一行代码生成list
print([x * x for x in range(1, 10)]) # [1, 4, 9, 16, 25, 36, 49, 64, 81]

# 生成list后再添加条件语句
print([x * x for x in range(1, 10) if x % 2 == 0]) # [4, 16, 36, 64]

# 还可以使用两层循环，可以生成全排列
print([x + y for x in 'ABC' for y in 'DEF']) # ['AD', 'AE', 'AF', 'BD', 'BE', 'BF', 'CD', 'CE', 'CF']

# 参数也可以使用dict
d = {'x': 'A', 'y': 'B', 'z': 'C' }
print([k + '=' + v for k, v in d.items()]) # ['x=A', 'y=B', 'z=C']


# 一行代码生成指定的list，这里可以看到 for 前的 if 是有 else 的 for 后的 if 是没有 else 且是不可以加的，是因为之前的 if else 是一个 表达式，之后的 if 是一个过滤条件 
print([x * x for x in range(1, 10) if x % 2 == 0]) # [4, 16, 36, 64]
print([x * x if x % 2 == 0 else -x * x for x in range(1, 10)]) # [-1, 4, -9, 16, -25, 36, -49, 64, -81]


# generator 生成器（迭代器），只要把一个列表生成式的[]改成()，就创建了一个generator
L = [x * x for x in range(10)] # list
G = (x * x for x in range(10)) # generator, 需要通过next(G) 打印出generator内容，generator保存的是算法，每次调用next(G)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误（一般来说都会用for循环generator，所以这个错误一般不用考虑）

# 如果一个函数包含 yield 那么这个函数就不是不同函数，而是 generator 函数
# 使用for循环时候是拿不到 generator 的 return 值的，如果想要拿到返回值，必须捕获 StopIteration 错误，返回值包含在 StopIteration 的 value 中

# isinstance 可以判断当前对象是否为Iterable 或者 Iterator
# 判断是否是一个Iterable对象 (可迭代对象)
# Iterable 类型包括：list、tuple、dict、set、str，generator
from collections.abc import Iterable
print(isinstance([], Iterable)) # True
print(isinstance({}, Iterable)) # True
print(isinstance('abc', Iterable)) # True
print(isinstance((x for x in range(10)), Iterable)) # True
print(isinstance(100, Iterable)) # False


# Iterator(迭代器)：可以被for循环而且可以被next()函数调用并不断返回下一个值的对象称为迭代器，知道抛出 StopIteration 表示无法返回下一个值了
from collections.abc import Iterator
print(isinstance((x for x in range(10)), Iterator)) # True
print(isinstance([], Iterator)) # False
print(isinstance({}, Iterator)) # False
print(isinstance('abc', Iterator)) # False

# 生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iterator。
# 把list、dict、str等Iterable变成Iterator可以使用iter()函数：
print(isinstance(iter([]), Iterator)) # True
print(isinstance(iter('abc'), Iterator)) # True


# 凡是可作用于for循环的对象都是Iterable类型
# 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列
```


## 函数式编程

1. 高阶函数

```python
# map 传入两个参数，一个是函数，一个是Iterable，即函数对象本身。由于结果r是一个Iterator，Iterator是惰性序列，因此通过list()函数让它把整个序列都计算出来并返回一个list
def f(x):
    return x * x

r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
print(list(r)) # [1, 4, 9, 16, 25, 36, 49, 64, 81]


# reduce reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算
from functools import reduce
def fn(x, y):
	return x * 10 + y

print(reduce(fn, [1, 3, 5, 7, 9])) # 13579


# 比如这样一个操作，将字符串13579转位数字13579
digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def transInt():
    def fn1(x, y):
        return x * 10 + y

    def char2(x):
        return digits[x]
    return reduce(fn1, map(char2, '13579'))


print(transInt(), isinstance(transInt(), int)) # 13579 True

# 以上可以用 lambda 函数进行简化
def char2num(s):
    return digits[s]

def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))

print(str2int('13579')) # 13579


# filter 接收一个函数和一个序列，把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
def is_odd(n):
    return n % 2 == 1

print(list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))) # [1, 5, 9, 15]
print(list(filter(lambda x: x % 2 == 0, [1, 2, 4, 5, 6, 9, 10, 15])))

# sorted 排序算法
# 比较只有数字的list
print(sorted([36, 5, -12, 9, -21])) # [-21, -12, 5, 9, 36]

# sorted第二个参数可以传递一个key，可以对排序的内容进行处理后再次排序
print(sorted([36, 5, -12, 9, -21], key=abs)) # [5, 9, -12, -21, 36]

# 如果需要进行反向排序，传递第三个参数即可
print(sorted(['Zoo', 'Credit', 'bob', 'about'], key=str.lower, reverse=True)) # ['Zoo', 'Credit', 'bob', 'about']

# 当然我们可以自定义key
L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
# 按照名字拍粗
def by_name(t):
    return t[0]

print(sorted(L, key=by_name))
# 按照成绩从高到低排序
def by_score(t):
    return -t[1]

print(sorted(L, key=by_score))



# 返回函数
# 我们在函数lazy_sum中又定义了函数sum，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量，当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum


f1 = lazy_sum(1, 3, 5, 7, 9)
f2 = lazy_sum(1, 3, 5, 7, 9)
print(f1 == f2) # False

# 闭包  返回函数不要引用任何循环变量，或者后续会发生变化的变量
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs


f1, f2, f3 = count()
print(f1(), f2(), f3()) # 9 9 9
# 返回内容全部都是9 原因在于返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了3，因此最终结果为9

# 怎样处理以上问题 
# 第一种：可以再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变
def count():
    def f(j):
        def g():
            return j * j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i))
    return fs


f1, f2, f3 = count()
print(f1(), f2(), f3()) # 1 4 9

# 第二种：nonlocal
def createCounter():
    x = 0
    def counter():
        # 使用闭包时，对外层变量赋值前，需要先使用nonlocal声明该变量不是当前函数的局部变量
        nonlocal x
        x = x + 1
        return x
    return counter

counterA = createCounter()
print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5



# 匿名函数 lambda
list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
    # 这里的lambda x: x * x 其实就等于
def f(x):
    return x * x

# 匿名函数有个限制，就是只能有一个表达式，不用写return，返回值就是该表达式的结果
# 当然也可以将匿名函数赋值给一个变量，再利用变量来调用该函数
f = lambda x: x * x
print(f(5)) # 25



# 装饰器
# 函数对象有一个__name__属性，可以拿到函数的名字
def now():
    print('拉面说')

f = now
print(now.__name__, f.__name__) # now now

# 假设我们要增强now()函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改now()函数的定义，这种在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）

# 日志装饰器
def log(func):
    def wrapper(*arg, **kw):
        print('call %s():' % func.__name__)
        return func(*arg, **kw)
    return wrapper

@log
def now():
    print('拉面说')
# @log 相当于执行了以下这行代码
# now = log(now)

now() # call now(): 拉面说
print(now.__name__) # wrapper

# 如果装饰器本身要传如一个参数，那么就需要在增加一层嵌套了
def log(text):
    def decorator(func):
        def wrapper(*arg, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*arg, **kw)
        return wrapper
    return decorator

@log('自定义内容')

# 可以看到上面的 now.__name__ 打印的是 wrapper 而不是当前函数 now，所以我们需要处理使用装饰器后的函数名字

import functools

def log(text):
    def decorator(func):
        # 将函数名归正
        @functools.wraps(func)
        def wrapper(*arg, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*arg, **kw)
        return wrapper
    return decorator

@log('自定义内容')
now() # 自定义内容 now(): 拉面说
print(now.__name__) # now



# 便函数 functools.partial，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单
import functools
int2 = functools.partial(int, base=2) # int(x,2)
print(int2('1000000')) # 64

max2 = functools.partial(max, 10) # max(10,)
print(max2(1,2,3,4,6)) # 10 max(10,1,2,3,4,6)
```


## 模块部分

1. 模块，一个abc.py的文件就是一个名字叫abc的模块，一个xyz.py的文件就是一个名字叫xyz的模块，假设我们的abc和xyz这两个模块名字与其他模块冲突了，于是我们可以通过包来组织模块，避免冲突。方法是选择一个顶层包名，比如mycompany，按照如下目录存放：

```
mycompany
├─ __init__.py
├─ abc.py
└─ xyz.py
```
    引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突。现在，abc.py模块的名字就变成了mycompany.abc，类似的，xyz.py的模块名变成了mycompany.xyz,每一个包目录下面都会有一个__init__.py的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。__init__.py可以是空文件，也可以有Python代码，因为__init__.py本身就是一个模块，而它的模块名就是mycompany


```python
# sys模块有一个argv变量，用list存储了命令行的所有参数
import sys

def test():
    args = sys.argv
    print(args)
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')
# 解释下以下两行代码 当我们在命令行运行model模块文件时，Python解释器把一个特殊变量__name__置为__main__，而如果在其他地方导入该hello模块时，if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试
if __name__=='__main__':
    test()

python3 model.py # ['model.py']
python3 model.py byron # ['model.py', 'byron']

```


2. 作用域
* 正常的变量是公开的可以被正常引用的，比如：abc，x123，PI等，
* 类似__xxx__这样的变量是特殊变量，可以被直接引用，但是有特殊用途，我们自己的变量一般不要用这种变量名
* 类似_xxx和__xxx这样的函数或变量就是非公开的（private），不应该被直接引用，比如_abc，__abc等


3. 第三方模块，Python中，安装第三方模块，是通过包管理工具pip（pip3）完成的



## 面向对象

1. 类跟实例

```python
# Student 为类名，(object)表示该类是从哪个类继承下来的
class Student(object):
    # __init__ 类似于js的 constructor，进行初始化
    def __init__(self, name, score):
        self.name = name
        self.score = score
    
     def print_score(self, test):
        print('我叫 %s 我今年 %d 岁. test' % (self.name, 
        self.score))

# 实力化
bart = Student('byron', 24)
bart.print_score(‘test’) # 我叫 byron 我今年 24 岁. test
print(bart.name, bart.score) # byron 24
```

2. 访问限制，如上代码，实例化一个类后， 我们可以直接调用或修改内部的属性，但是一般来说内部属性是不能够在外部随便调用的

```python
class Student(object):
    # __init__ 类似于js的 constructor，进行初始化
    def __init__(self, name, score):
        # 变为私有变量，只有内部可以访问
        self.__name = name
        self.__score = score
    
     def print_score(self):
        print('我叫 %s 我今年 %d 岁' % (self.__name, 
        self.__score))
    # 外部获取内部变量
    def get_name(self):
        return self.__name
    # 外部修改内部变量
    def set_name(self, name):
        # 这里可以对赋值参数做一些处理，掌控主动权
        self.__name = name


print(bart.__name) # 'Student' object has no attribute '__name'
# 私有变量其实可以这样访问，是因为解释器把 __name 变成了 _Student__name，但是不同的解释器可能会变成的名字不同，所以 强烈不建议这么搞
# print(bart._Student__name)
```


3. 继承跟多态

```python
class Animal(object):
    def run(self):
        print('Animal is running...')
    
    def eat(self):
        print('Animal Eating meat...')


class Dog(Animal):
    def eat(self):
        print('Dog Eating meat...')

# 对于Dog来说，Animal就是它的父类，对于Animal来说，Dog就是它的子类，子类可以继承父类的方法（这就是继承）
dog = Dog()
dog.run() # Animal is running...
# 当子类和父类都存在相同的方法时候，子类会覆盖父类的方法（这就是多态）
dog.eat() # Dog Eating meat...

a = list()
b = Animal()
c = Dog()
print(isinstance(a, list)) # True
print(isinstance(b, Animal)) # True
print(isinstance(b, Dog)) # False Dog可以看成Animal，但Animal不可以看成Dog
print(isinstance(c, Dog)) # True
print(isinstance(c, Animal)) # True

# 继承可以把父类的所有功能都直接拿过来，这样就不必重零做起，子类只需要新增自己特有的方法，也可以把父类不适合的方法覆盖重写

# 动态语言的鸭子类型特点决定了继承不像静态语言那样是必须的

```

4. 获取对象的信息

* type 基本类型都可以用 type 来进行判断

```python
print(type(123)) # <class 'int'>
print(type('123')) # <class 'str'>
print(type(None)) # <class 'NoneType'>
print(type(abs)) # <class 'builtin_function_or_method'>
b = Animal()
print(type(b)) # <class '__main__.Animal'>
print(type(type('123'))) # <class 'type'>
print(type(123)==int) # True
print(type('abc')==str) # True

# 判断基本数据类型可以直接写int，str等，但如果要判断一个对象可以使用types模块中定义的常量
import types

def fn():
    pass

print(type(fn)==types.FunctionType) # True
print(type(abs)==types.BuiltinFunctionType) # True
print(type(lambda x: x)==types.LambdaType) # True
print(type((x for x in range(10)))==types.GeneratorType) # True

```

* isinstance()：优先使用

```python
print(isinstance('a', str)) # True
print(isinstance(123, int)) # True
print(isinstance(b'a', bytes)) # True
# 判断是否为某一种类型
print(isinstance([1, 2, 3], (list, tuple))) # True
# 判断类可以向上看

```

* dir()，获得一个对象的所有属性和方法

```python
dir('ABC')
# ['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']

# 类似__xxx__的属性和方法在Python中都是有特殊用途的，比如__len__方法返回长度。在Python中，如果你调用len()函数试图获取一个对象的长度，实际上，在len()函数内部，它自动去调用该对象的__len__()方法

# len('ABC') == 'ABC'.__len__()

# getattr()、setattr()以及hasattr()，我们可以直接操作一个对象的状态

class MyDog(object):
    def __init__(self):
        self.x = 1

print(hasattr(dog, 'x')) # True
setattr(dog, 'x', 123) # 将dog中的x设置123
print(hasattr(dog, 'y')) # False
print(getattr(dog, 'x')) # 123
print(dog.x) # 123
print(getattr(dog, 'z')) # error AttributeError
print(getattr(dog, 'z', 404)) # 404 如果没有找到，那么就用默认值
```

* 实例属性和类属性，编程过程中，千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性



## 面向对象高级编程

1. 使用__slots__

```python
# 给一个实例添加一个方法
class Student(object):
    pass

s = Student()

def set_age(self, name):
    self.name = name

from types import MethodType
# 使用 MethodType 给实例添加方法
s.set_age = MethodType(set_age, s)
s.set_age(123)
print(s.name) # 123

# Student.set_score = set_score 类中可以直接添加方法

# 限制实例属性 __slots__
class Student(object):
    __slots__ = ('name', 'age')

s = Student()
s.name = 'byron'
s.age = 321
# s.score = 123123 # error AttributeError

# __slots__ 对继承的子类是无效的
class GraduateStudent(Student):
    pass

g = GraduateStudent()
g.score = 123

```

2. @property，可以优雅的给变量添加 get、set方法

```python
class Student(object):
    # 添加 @property 就是将score设置成一个get方法，类似与装饰器
    @property
    def score(self):
        return self._score

    # 添加 @score.setter 就是将setter设置成一个set方法
    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        self._score = value

    # 如果只有@property，那么就是一个只读的属性，不能写
    @property
    def age(self):
        return 2015 - self._birth

    # 要特别注意：属性的方法名不要和实例变量重名，因为调用s.birth时，首先转换为方法调用，在执行return self.birth时，又视为访问self的属性，于是又转换为方法调用，造成无限递归，最终导致栈溢出报错RecursionError
    # @property
    # def birth(self):
        # return self.birth


s = Student()
s.score = 123 # 转化为s.set_score(60)
print(s.score) # 转化为s.get_score()
s.score = '9999' # ValueError: score must be an integer!

```

3. 多重继承

```python
class Animal(object):
    def animate(self):
        print('animate...')

class Runnable(object):
    def run(self):
        print('Running...')

class Dog(Animal, Runnable):
    pass

d = Dog()
d.animate() # animate...
d.run() # Running...

```

4. 定制类

```python
# __str__：优化打印类的内容
class Student(object):
    def __init__(self, name):
        self._name = name
        
    def __str__(self):
        return 'Student object (name: %s)' % self._name
    # __str__()返回用户看到的字符串，而__repr__()返回程序开发者看到的字符串，也就是说，__repr__()是为调试服务的
    __repr__ = __str__


# 添加__str__前
print(Student('byron')) # <__main__.Student object at 0x103fbef10>
# 添加__str__后
print(Student('byron')) # Student object (name: byron)


# __iter__：返回一个迭代对象，然后会不断调用 __next__ 方法获取下一个值
# __getitem__：可以实现类似 list 一样，去除对应的元素
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
    
    def __getitem__(self, item):
        # 处理单独获取下标方法
        if isinstance(item, int):
            a, b = 1, 1
            for key in range(a, item):
                a, b = b, a + b
            return a
        # 处理list的切片方法，但是没有处理step以及负数
        if isinstance(item, slice): # 判断当时是否为切片
            start = item.start
            stop = item.stop
            if start is None:
                start = 0
            a, b = 1, 1
            ss = []
            for key in range(stop):
                if key >= start:
                    ss.append(a)
                a, b = b, a + b
            return ss


for item in Fib():
    print(item)

print(Fib()[5]) # 5
print(Fib()[0:5]) # [1, 1, 2, 3, 5]


# __getattr__：如果调用不存在的值，那么就会尝试调用该方法
class Student(object):
    def __init__(self):
        self.name = 'byron'

    def __getattr__(self, item):
        if item == 'score':
            return 999
        if item == 'age':
            return lambda: 25
        # 最后如果不加 AttributeError 就会返回 None，一般来说类如果找不到属性那么就会报错 AttributeError，所以要加这行return
        return AttributeError('\'Student\' object has no attribute \'%s\'' % item)


s = Student()
print(s.name) # byron
print(s.score) # 999
print(s.age()) # 25
print(s.a) # 'Student' object has no attribute 'a'


# __call__：类是一个可调用的，但是实例化又是一个不可调用的，当我们在类中加上__call__，那么实例化就变成可调用的了
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)


s = Student('byron')
s() # My name is byron.
# callable 判断当前是否为 可调用对象
print(callable(Student)) # True
print(callable(s)) # True

# 小案例，根据链式调用返回对应的地址
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __call__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__


print(Chain().status.user.timeline.list) # /status/user/timeline/list
print(Chain().users('michael').timeline.list) # /users/michael/timeline/list

```

5. 使用枚举类

```python
from enum import Enum, unique
# 第一种枚举方式
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
# 枚举单个成员
print(Month.Jan) # Month.Jan
# 枚举所有成员
for name, member in Month.__members__.items():
    # member.value 从 1 开始的
    print(name, '=>', member, ',', member.value) # Jan => Month.Jan , 1

# 第二种枚举方式
# @unique装饰器可以帮助我们检查保证没有重复值
@unique
class WeekDay(Enum):
    Sun = 0  # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6

print(WeekDay.Sun) # WeekDay.Sun
print(WeekDay.Sun.name, WeekDay.Sun.value) # Sun 0
print(WeekDay(1)) # WeekDay.Mon
for name, member in WeekDay.__members__.items():
    # member.value 从 0 开始
    print('%s => %s %d' %(name, member, member.value)) # Sun => WeekDay.Sun 0

```

6. 使用元类
* type

```python
# type()函数可以查看一个类型或变量的类型，也可以创建一个类，对没错，就不需要 class Hello(object)... 来创建，从而实现动态创建一个类

def fn(self, name = 'world'):
    print('Hello %s' % name)

# 要创建一个class对象，type()函数依次传入3个参数：
# 1. class的名称；
# 2. 继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
# 3. class的方法名称与函数绑定，这里我们把函数fn绑定到方法名hello上

Hello = type('Hellp', (object,), dict(hello=fn))
h = Hello()
h.hello() # Hello world
# 通过type()函数创建的类和直接写class是完全一样的，因为Python解释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用type()函数创建出class

```

* metaclass

(看这里吧)[https://www.liaoxuefeng.com/wiki/1016959663602400/1017592449371072#0]


```python
# 除了使用type()动态创建类以外，要控制类的创建行为，还可以使用metaclass。

```


## 错误、调试和测试

1. 错误处理 try...except...finally...

```python
try:
    print('try...')
    r = 10 / 0
    print('result:', r)
# 如果先补货 BaseException 那么下面的错误就捕获不到了，因为BaseException 是整个错误的基类
# except BaseException as e:
    # print('BaseException:', e)    
# 这里可以多次使用except进行捕获错误
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
else:
    print('no error!')
finally:
    print('finally...')

# try...
# ZeroDivisionError: division by zero
# finally...

# Python的错误其实也是class，所有的错误类型都继承自BaseException，所以在多层捕获需要注意错误的父类子类关系，如果先不过一个父类再捕获一个子类，那么这个子类永远也不会捕获到

```

2. 调试

* print 打log，类似 js 的 console.log

* 断言，凡是用print()来辅助查看的地方，都可以用断言（assert）来替代

```python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')

main() # AssertionError: n is zero!

# 如果代码中充斥着 assert 的话跟 print 也差不多，最后也是要删除。但是Python解释器时可以用-O参数来关闭assert
# python -O err.py

```

* logging：与 print，assert 相比 logging不会抛出错误，而且可以输出到文件中（建议）

```python
import logging
# 可以控制我们要输出信息的级别，有debug，info，warning，error等几个级别，当我们指定level=INFO时，logging.debug就不起作用了。同理，指定level=WARNING后，debug和info就不起作用了
logging.basicConfig(level=logging.INFO)

s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)

```

* pdb：可以让程序进行单步调试（很麻烦）

```python
s = '0'
n = int(s)
print(10 / n)

$ python -m pdb err.py # 通过 -m pdb 开启单步调试
l # 查看代码
n # 进行单步操作
p s # 查看 s 变量的内容
q # 退出调试

```

* pdb.set_trace()：这个方法也是用pdb，但是不需要单步执行，我们只需要import pdb，然后，在可能出错的地方放一个pdb.set_trace()，就可以设置一个断点，断点调试

```python
import pdb

s = '0'
n = int(s)
# 当我们执行代码后，会自动在这行暂停进行 pdb 调试环境
pdb.set_trace()
print(10 / n)

$ p s # 查看变量
$ c # 继续进行运行

```

* 还可以选择 IDE 调试



## IO

1. 文件读写

```python
address = '/Users/byron/Desktop/excel.js'
text = '/Users/byron/Desktop/text.txt'

try:
    # 打开文件 
    # r 代表读取、rb 代表二进制文件（图片）
    # w 代表写、wb 代表写二进制文件、a 代表追加写
    f = open(address, 'r')
finally:
    if f:
        # 以字符串形式查看当前文件内容
        print(f.read())
        # 关闭文件
        f.close()


# 由于每次都要操作 close 关闭文件，会很繁琐，所以可以使用 with 帮助我们自定关闭文件
with open(address, 'r') as f:
    # 读取全部内容
    print(f.read())
    # 读取 100 字节的内容
    print(f.read(100))
    # 读取一行的内容
    print(f.readline())
    # 一次读取所有内容并以 list 形式返回
    print(f.readlines())

# 如果文件很小，read()一次性读取最方便；如果不能确定文件大小，反复调用read(size)比较保险；如果是配置文件，调用readlines()最方便

# 字符编码，比如读取非UTF-8编码的文本文件，需要传递encoding='gbk'来选择对应的编码格式
# errors='ignore'，代表遇到错误忽略
f = open('...', 'r', encoding='gbk', errors='ignore')

# 写文件，如果没有该文件会自动创建
with open(text, 'w', encoding='utf-8') as f:
    f.write('测试')

```

2. StringIO和BytesIO

* StringIO：内存中读写str

```python
from io import StringIO

f = StringIO()
f.write('hello')
f.write(' ')
f.write('python')
print(f.getvalue()) # hello python

f = StringIO('123')
print(f.read()) # 123

```

* BytesIO：内存中读写二进制数据

```python
from io import BytesIO
f = BytesIO()
f.write('哈喽'.encode('utf-8'))
print(f.getvalue()) # b'\xe5\x93\x88\xe5\x96\xbd'

f = BytesIO('哈喽'.encode('utf-8'))
print(f.read()) # b'\xe5\x93\x88\xe5\x96\xbd'

```

3. 操作文件和目录

```python
# 操作系统类型
print(os.name) # posix，说明系统是Linux、Unix或Mac OS X，如果是nt，就是Windows系统
print(os.uname()) # 详细信息，windos 不支持
print(os.environ) # 获取环境变量
print(os.environ.get('PATH')) # 获取指定环境变量
# 查看当前目录的绝对路径
print(os.path.abspath('.')) # /Users/byron/Documents/dev/demo/pythondemo
# 路径拼加
print(os.path.join('/Users/byron/Desktop', 'byron')) # /Users/byron/Desktop/byron
os.mkdir(os.path.join('/Users/byron/Desktop', 'byron')) # 创建文件，如果文件存在报错
os.rmdir(os.path.join('/Users/byron/Desktop', 'byron')) # 删除文件
# 拆分路径
print(os.path.split('/Users/byron/Desktop/text.txt')) # ('/Users/byron/Desktop', 'text.txt')
# 拆分后缀
print(os.path.splitext('/Users/byron/Desktop/text.txt')) # ('/Users/byron/Desktop/text', '.txt')
# 重命名
os.rename('/Users/byron/Desktop/text.txt', '/Users/byron/Desktop/hah.py')
# 删除文件
os.remove('/Users/byron/Desktop/hah.py')
# 查看当前是否为文件夹
print(os.path.isdir('/Users/byron/Desktop')) # True
# 查看当前是否为一个文件
print(os.path.isfile('/Users/byron/Desktop/excel.js')) # True
# 以 list 返回当前路径下的文件
print(os.listdir('.'))

```

4. 序列化

* pickle

```python
import pickle
d = dict(name='Bob', age=20, score=88)
print(d) # {'name': 'Bob', 'age': 20, 'score': 88}
# pickle.dumps()方法把任意对象序列化成一个bytes
print(pickle.dumps(d)) # b'\x80\x04\x95$\x...
# pickle.loads()方法反序列化出对象
print(pickle.loads(pickle.dumps(d))) # {'name': 'Bob', 'age': 20, 'score': 88}

with open('/Users/byron/Desktop/a.txt', 'wb') as f:
    # 将序列化二进制存入文件，注意这里是 dump
    pickle.dump(d, f)

with open('/Users/byron/Desktop/a.txt', 'rb') as f:
    # 反序列化，注意这里是 load
    c = pickle.load(f)
    print(c) # {'name': 'Bob', 'age': 20, 'score': 88}

```

* json

```python
import json
d = dict(name='Bob', age=20, score=88)
# json.dumps 将dict转为str
print(json.dumps(d), isinstance(json.dumps(d), str)) # {"name": "Bob", "age": 20, "score": 88} True
json_str = json.dumps(d)
# json.loads 将str转为dict
print(json.loads(json_str), isinstance(json.loads(json_str), dict)) # {'name': 'Bob', 'age': 20, 'score': 88} True

with open('/Users/byron/Desktop/a.json', 'w') as f:
    json.dump(d, f)

with open('/Users/byron/Desktop/a.json', 'r') as f:
    c = json.load(f) # {'name': 'Bob', 'age': 20, 'score': 88}
    print(c)


# json 序列化类 第一种方法
import json

class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

s = Student('Bob', 20, 88)

# print(json.dumps(s)) # Object of type Student is not JSON serializable

def student2dict(self):
    return {
        "name": self.name,
        "age": self.age,
        "score": self.score
    }

# 这样操作相当于把类先转成一个 dict，然后在 json 序列化
print(json.dumps(s, default=student2dict)) # {"name": "Bob", "age": 20, "score": 88}


# json 序列化类 第二种方法
# 直接调用类的__dict__方法将类转化成一个dict
print(json.dumps(s, default=lambda obj: obj.__dict__)) # {"name": "Bob", "age": 20, "score": 88}

# json 反序列化为 类
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])

class_str =  json.dumps(s, default=student2dict) # 将类序列化为str
class_dict = json.loads(class_str) # 将str反序列化为 dict
# 将 dict 反序列化为 class
print(dict2student(class_dict)) # <__main__.Student object at 0x10c1badc0>


# json.dumps(obj, ensure_ascii=False) # ensure_ascii 是否将数据进行ascii化

```


## 进程和线程

* 进程：对于操作系统来说，一个任务就是一个进程（Process），比如打开一个浏览器就是启动一个浏览器进程，打开一个记事本就启动了一个记事本进程，打开两个记事本就启动了两个记事本进程，打开一个Word就启动了一个Word进程

* 线程：有些进程还不止同时干一件事，比如Word，它可以同时进行打字、拼写检查、打印等事情。在一个进程内部，要同时干多件事，就需要同时运行多个“子任务”，我们把进程内的这些“子任务”称为线程（Thread）

* 线程是最小的执行单元，而进程由至少一个线程组成。如何调度进程和线程，完全由操作系统决定，程序自己不能决定什么时候执行，执行多长时间。

1. 多进程

```python
# 1. fork
import os
print('Process (%s) start...' % os.getpid())
# 调用一次，返回两次，因为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回
pid = os.fork()
# fork出的子进程永远返回0，，父进程返回子进程的id
if pid == 0:
    print('我是子进程 (%s) 我的父进程为 %s.' % (os.getpid(), os.getppid()))
else:
    print('我是 (%s) 进程，创建了一个子进程 (%s).' % (os.getpid(), pid))


# 2. 如果要启用大量子进程，可以用线程池的方法创建
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')

# Pool对象调用join()方法会等待所有子进程执行完毕，调用join()之前必须先调用close()，调用close()之后就不能继续添加新的Process了


# 3. 子进程，subprocess模块可以让我们非常方便地启动一个子进程，然后控制其输入和输出
import subprocess

print('$ nslookup www.fbyron.cn')
r = subprocess.call(['nslookup', 'www.fbyron.cn'])
print('Exit code:', r)


# 4. 进程间通信
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()

```

2. 多线程

```python
# 1. 启动一个线程
import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

# threading.current_thread() 返回主线程的实例
print('thread %s is running...' % threading.current_thread().name) # MainThread 主线程的名字
# 启动一个线程就是把一个函数传入并创建Thread实例，然后调用start()开始执行，name表示线程的名字
t = threading.Thread(target=loop, name='LoopThread') #
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)


# 多进程与多线程：在多进程中，同一个变量，各自会拷贝一份然后进行使用，互不影响；在多线程中，大家共享同一个变量，所以任何一个变量都有可能被任何一个线程所改变，还是很危险的，所以需要确保一个线程在修改变量时，其他的线程是不能修改的，可以通过 threading.Lock() 添加锁来实现

import time, threading

# 创建一个锁
lock = threading.Lock()
# 假定这是你的银行存款:
balance = 0

def change_it(n):
    # 先存后取，结果应该为0:
    global balance
    balance = balance + n
    balance = balance - n


def run_thread(n):
    for i in range(900000):
        # 不加锁，理论上说实行完打印内容应该为0，但是不加锁很有可能不是0，可以试试看
        # change_it(n)
        # 添加锁
        # 先要获取锁，当多个线程同时执行lock.acquire()时，只有一个线程能成功地获取锁，然后继续执行代码，其他线程就继续等待直到获得锁为止
        lock.acquire()
        try:
            # 放心地改吧，这里 try 锁因为要保证之后一定要释放锁，不然后面的线程一直在等待就成了死线程
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()

print(time.time())
t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)
print(time.time())

```

3. ThreadLocal：在多线程中优雅的创建全局数据

```python
import threading
    
# 创建全局ThreadLocal对象:
local_school = threading.local()

def process_student():
    # 获取当前线程关联的student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定ThreadLocal的student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',), name='Thread-A')
t2 = threading.Thread(target= process_thread, args=('Bob',), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()

# Hello, Alice (in Thread-A)
# Hello, Bob (in Thread-B)

```

4. 分布式进程：https://www.liaoxuefeng.com/wiki/1016959663602400/1017631559645600


## 正则匹配

1. re模块：Python提供re模块，包含所有正则表达式的功能

```python
import re

str = r'ABC\-001' # 建议字符串前面用 r 避免转义问题
# re.match(正则表达式，字符串)
print(re.match(r'^\d{3}\-\d{3,8}$', '123-12312311')) # <re.Match object; span=(0, 12), match='123-12312311'>
print(re.match(r'^\d{3}\-\d{3,8}$', '0123-12312311')) # None

```

2. 切分字符串

```python
import re
print('a b   c'.split(' ')) # ['a', 'b', '', '', 'c']
print(re.split(r'\s+', 'a b   c')) # ['a', 'b', 'c']  可以过滤字符串的空格
print(re.split(r'[\s\,\;]+', 'a,b  ;c;; d')) # ['a', 'b', 'c', 'd']

```

3. 分组

```python
import re
m = re.match(r'^(\d{3})-(\d{3,8})$', '010-12345')
print(m.groups()) # ('010', '12345')
print(m[0]) # 010-12345
print(m[1]) # 010
print(m[2]) # 12345

```

4. 贪婪匹配

```python
# 由于 (\d+) 会尽可能的多匹配，所以会匹配全部字符串，导致 (0*) 没有内容可匹配
print(re.match(r'^(\d+)(0*)$', '102300').groups()) # ('102300', '')
# 我们使用 (\d+?) 采用非贪婪匹配，可以让 (0*) 有内容可以匹配
print(re.match(r'^(\d+?)(0*)$', '102300').groups()) # ('1023', '00')

```

5. 编译，如果一个正则我们需要使用很多次，那么我们就可以使用预编译在进行使用，从而减少每次在使用过程中的编译问题

```python
import re
# 预编译
re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
# 使用
print(re_telephone.match('010-12345').groups()) # ('010', '12345')

```


## 常用模块

1. datetime：Python处理日期和时间的标准库

```python
from datetime import datetime, timedelta
# 获取当前datetime
print(datetime.now()) # 2022-06-12 16:29:40.682467
print(type(datetime.now())) # <class 'datetime.datetime'>
# 返回指定时间
print(datetime(2022, 6, 12, 12, 20)) # 2022-06-12 12:20:00
# 返回时间戳 秒
print(datetime.now().timestamp(), datetime(2022, 6, 12, 12, 20).timestamp()) # 1655022894.105519 1655007600.0
# 时间戳转时间
print(datetime.fromtimestamp(1655022894.105519)) # 2022-06-12 16:34:54.105519
# 转为 UTC 时间
print(datetime.utcfromtimestamp(1655022894.105519)) # 2022-06-12 08:34:54.105519
# str转换为datetime
print(datetime.strptime('2022-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')) # 2022-06-01 18:19:59
# datetime转换为str
print(datetime.now().strftime('%Y-%m-%d %H:%M:%S'), datetime.now().strftime('%a, %b %d %H:%M')) # 2022-06-12 16:42:15 Sun, Jun 12 16:42
# datetime加减
print(datetime.now() + timedelta(hours=2)) # 计算当前时间的两小时后
print(datetime.now() + timedelta(days=2)) # 计算当前时间两天后
print(datetime.now() + timedelta(days=2, hours=2)) # 计算当前时间两天两小时后

```

2. collections：Python内建的一个集合模块，提供了许多有用的集合类

* namedtuple：用来创建一个自定义的tuple对象，并且规定了tuple元素的个数，并可以用属性而不是索引来引用tuple的某个元素
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(100, 200)
print(p.x, p.y) # 100 200
print(isinstance(p, Point)) # True
print(isinstance(p, tuple)) # True

```

* deque：可以高效实现插入和删除操作的双向列表，适合用于队列和栈

```python
from collections import deque

d = deque(['a', 'b', 'c'])
d.append('d') # 尾部插入一个元素
d.appendleft('o') # 头部插入一个元素
# d.pop() # 删除尾部一个元素
# d.popleft() # 删除头部一个元素
print(d) # deque(['o', 'a', 'b', 'c', 'd'])

```

* defaultdict：在使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict

```python
from collections import defaultdict

# 默认值是调用函数返回的，而函数在创建defaultdict对象时传入
d = defaultdict(lambda: 'N/A')
d['key1'] = 'key1'
print(d['key1']) # key1
print(d['key2']) # N/A

```

* OrderedDict：在使用dict时，Key是无序的。在对dict做迭代时，我们无法确定Key的顺序，如果需要保证key的顺序，可以使用OrderedDict, **OrderedDict的Key会按照插入的顺序排列，不是Key本身排序**

```python
from collections import OrderedDict

# d = dict()
# d['c'] = 1
# d['a'] = 2
# d['b'] = 3
# for item, key in d.items():
#     print(item, key)
# 还不是很明白这个排序是有什么作用😱😱😱
od = OrderedDict()
od['c'] = 1
od['a'] = 2
od['b'] = 3
print(od) # OrderedDict([('c', 1), ('a', 2), ('b', 3)])
print(od.keys()) # odict_keys(['c', 'a', 'b'])

```

* ChainMap：可以把一组dict串起来并组成一个逻辑上的dict

```python
from collections import ChainMap
import os, argparse

# 构造缺省参数:
defaults = {
    'color': 'red',
    'user': 'guest'
}

# 构造命令行参数: 可以在命令行添加参数 eg：python3 use_chainmap.py -u bob -c blue
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args()
command_line_args = {k: v for k, v in vars(namespace).items() if v}
print(command_line_args)
# 1.如果命令行没有输入那么打印出 {}
# 2.如果命令行这样输入：python3 use_chainmap.py -u bob -c blue，那么打印出 {'user': 'byron', 'color': 'blue'}

# 组合成ChainMap, 这里会先查看 command_line_args（命令行）的参数，然后在查看 os.environ（环境变量），最后在看默认参数
combined = ChainMap(command_line_args, os.environ, defaults)

# 打印参数:
print('color=%s' % combined['color'], 'user=%s' % combined['user'])
# 1. 如果直接运行：color=red user=guest
# 2. 如果命令行构造参数运行 python3 inMode.py -u bob -c blue：color=blue user=bob
# 3. 如果环境变量参数&命令行构造参数运行  user=admin color=blue python3 inMode.py -u bob ：color=blue user=bob

```

* Counter：一个简单的计数器，例如，统计字符出现的个数，厉害

```python
from collections import Counter

c = Counter()
for item in 'byronhellopython':
    c[item] = c[item] + 1

print(c) # Counter({'o': 3, 'y': 2, 'n': 2, 'h': 2, 'l': 2, 'b': 1, 'r': 1, 'e': 1, 'p': 1, 't': 1})

```

3. base64：是一种用64个字符来表示任意二进制数据的方法

```python
import base64

# base64 编码
print(base64.b64encode(b'binary\x00string')) # b'YmluYXJ5AHN0cmluZw=='
# base64 解码
print(base64.b64decode(b'YmluYXJ5AHN0cmluZw==')) # b'binary\x00string'

# 由于标准的Base64编码后可能出现字符+和/，在URL中就不能直接作为参数，所以又有一种"url safe"的base64编码，其实就是把字符+和/分别变成-和_
print(base64.b64encode(b'i\xb7\x1d\xfb\xef\xff')) # b'abcd++//'
print(base64.urlsafe_b64decode(b'abcd++//')) # b'i\xb7\x1d\xfb\xef\xff'
print(base64.urlsafe_b64encode(b'i\xb7\x1d\xfb\xef\xff')) # b'abcd--__'
print(base64.urlsafe_b64decode(b'abcd--__')) # b'i\xb7\x1d\xfb\xef\xff'

# Base64适用于小段内容的编码，比如数字证书签名、Cookie的内容等

```

4. struct：处理bytes和其他二进制数据类型的转换

```python
import struct
# pack函数把任意数据类型变成bytes，'>I'：>表示字节顺序是big-endian，也就是网络序，I表示4字节无符号整数，后面的参数个数要和处理指令一致
print(struct.pack('>I', 10240099)) # b'\x00\x9c@c'
# unpack把bytes变成相应的数据类型，'>IH'：后面的bytes依次变为I：4字节无符号整数和H：2字节无符号整数
print(struct.unpack('>IH', b'\xf0\xf0\xf0\xf0\x80\x80')) # (4042322160, 32896)

```

5. hashlib：提供了常见的摘要算法（哈希算法、散列算法），如MD5，SHA1等等

```python
import hashlib

md5 = hashlib.md5()

md5.update('hello'.encode('utf-8'))
print(md5.hexdigest()) # 5d41402abc4b2a76b9719d911017c592

# 如果把内容拆开再去 md5 拿到的值也是一样的
md5.update('hel'.encode('utf-8'))
md5.update('lo'.encode('utf-8'))
print(md5.hexdigest()) # 5d41402abc4b2a76b9719d911017c592

sha1 = hashlib.sha1()

sha1.update('hello'.encode('utf-8'))
print(sha1.hexdigest()) # aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d

sha1.update('hel'.encode('utf-8'))
sha1.update('lo'.encode('utf-8'))
print(sha1.hexdigest()) # aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d

```

6. hmac：实现了标准的Hmac算法

```python
import hmac

message = b'Hello, world!'
key = b'secret'
# 传入的key和message都是bytes类型
h = hmac.new(key, message, digestmod='MD5')
print(h.hexdigest()) # fa4ee7d173f2d97ee79022d1a7355bcf

```

7. itertools：提供了非常有用的用于操作迭代对象的函数

```python
import itertools

# count 创建一个无限迭代器，会打印出所有的自然序列, 第一个参数为开始数子，第二个为间隔
natuals = itertools.count(1, 2)
for item in natuals:
    print(item)

# takewhile 可以根据条件判断截取出来一个有限的序列
ns = itertools.takewhile(lambda x: x <= 10, natuals)
print(list(ns)) # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# cycle 创建一个无限迭代器，将传入内容无限打印下去
cs = itertools.cycle('abc')
for item in cs:
    print(item)

# repeat 可以把一个元素无限重复下去，第二个参数是重复次数
re = itertools.repeat('byron', 10)
for item in re:
    print(item)

# chain 可以将一组迭代对象串联起来，形成一个更大的迭代器
ch = itertools.chain('ABC', 'DEF', 'GHI')
for item in ch:
    print(item) # A B C D E F G H I

# groupby 把迭代器中相邻的重复元素挑出来放在一起，第一个参数为重复的元素，第二个参数为一个函数来处理第一个元素
gr = itertools.groupby('AAAbBBBCccCAAAaa', lambda c: c.upper())
for k, v in gr:
    print(k, list(v))
    # A['A', 'A', 'A']
    # B['b', 'B', 'B', 'B']
    # C['C', 'c', 'c', 'C']
    # A['A', 'A', 'A', 'a', 'a']

```

8. contextlib：上下文

```python
# with 并不是只有在用 open 的时候可以使用，实际上，任何对象，只要正确实现了上下文管理，就可以用于with语句
# 实现上下文管理是通过__enter__和__exit__这两个方法实现的

class Query(object):
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        print('Begin')
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type:
            print('Error')
        else:
            print('END')

    def query(self):
        print('this is name %s' %self.name)

with Query('byron') as q:
    q.query()
    
# Begin
# this is name byron
# END


# 也可以使用 contextmanager 装饰器，更方便一点，而且我们有时候需要在指定代码前后打印内容，也可以使用这个装饰器
# @contextmanager这个decorator接受一个generator，用yield语句把with ... as var把变量输出出去，然后，with语句就可以正常地工作了
from contextlib import contextmanager

class Query(object):
    def __init__(self, name):
        self.name = name

    def query(self):
        print('this is name %s' %self.name)

@contextmanager
def create_query(name):
    print('Begin')
    q = Query(name)
    yield q
    print('End')

with create_query('byron') as q:
    q.query()

# Begin
# this is name byron
# End

# 这是一个在指定代码片段前后打印出内容的例子
@contextmanager
def tag(tag):
    print("<%s>" % tag)
    yield
    print("</%s>" % tag)

with tag('html'):
    print('start')
    print('end')

# <html>
# start
# end
# </html>


# @closing 如果一个对象没有实现上下文，我们就不能把它用于with语句。这个时候，可以用closing()来把该对象变为上下文对象
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen('http://www.fbyron.cn')) as page:
    for line in page:
        print(line)

```

9. urllib：提供了一系列用于操作URL的功能

```python
from urllib import request, parse

# GET 请求
    # 直接抓取页面
with request.urlopen('https://react.docschina.org/') as f:
    data = f.read()
    print('Status:', f.status)
    print('Reason:', f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', data.decode('utf-8'))

    # 添加请求头抓去页面
req = request.Request('https://react.docschina.org/')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
with request.urlopen(req) as f:
    data = f.read()
    print('Status:', f.status)
    print('Reason:', f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', data.decode('utf-8'))


# POST 请求，需要把参数data以bytes形式传入

    # 输入信息
email = input('Email: ')
passwd = input('Password: ')
    # 对信息进行bytes化
login_data = parse.urlencode([
    ('username', email),
    ('password', passwd),
    ('entry', 'mweibo'),
    ('client_id', ''),
    ('savestate', '1'),
    ('ec', ''),
    ('pagerefer', 'https://passport.weibo.cn/signin/welcome?entry=mweibo&r=http%3A%2F%2Fm.weibo.cn%2F')
])
    # 设置请求地址
req = request.Request('https://passport.weibo.cn/sso/login')
    # 添加请求头
req.add_header('Origin', 'https://passport.weibo.cn')
req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
req.add_header('Referer', 'https://passport.weibo.cn/signin/login?entry=mweibo&res=wel&wm=3349&r=http%3A%2F%2Fm.weibo.cn%2F')
    # 开启请求
with request.urlopen(req, data=login_data.encode('utf-8')) as f:
    print('Status:', f.status, f.reason)
    for k, v in f.getheaders():
        print('%s: %s' % (k, v))
    print('Data:', f.read().decode('utf-8'))


# 通过代理进行请求地址
proxy_handler = request.ProxyHandler({'http': 'http://www.example.com:3128/'})
proxy_auth_handler = request.ProxyBasicAuthHandler()
proxy_auth_handler.add_password('realm', 'host', 'username', 'password')
opener = request.build_opener(proxy_handler, proxy_auth_handler)
with opener.open('http://www.example.com/login.html') as f:
    pass

```

10. XML
* 操作XML有两种方法：DOM和SAX。DOM会把整个XML读入内存，解析为树，因此占用内存大，解析慢，优点是可以任意遍历树的节点。SAX是流模式，边读边解析，占用内存小，解析快，缺点是我们需要自己处理事件，正常情况下，优先考虑SAX，因为DOM实在太占内存

```python
from xml.parsers.expat import ParserCreate

class DefaultSaxHandler(object):
    # start_element事件，在读取<a href="/">时；
    def start_element(self, name, attrs):
        print('start_element: %s, attrs: %s' % (name, str(attrs)))

    # end_element事件，在读取</a>时
    def end_element(self, name):
        print('end_element: %s' % name)

    # char_data事件，在读取python时
    def char_data(self, text):
        print('char_data: %s' % text)

xml = r'''<?xml version="1.0"?>
<a href="/">python</a>
'''

handler = DefaultSaxHandler()
parser = ParserCreate()
parser.StartElementHandler = handler.start_element
parser.EndElementHandler = handler.end_element
parser.CharacterDataHandler = handler.char_data
print(parser.Parse(xml))

```

11. HTMLParser：继续HTMl

```python
from html.parser import HTMLParser
from html.entities import name2codepoint

class MyHTMLParser(HTMLParser):

    def handle_starttag(self, tag, attrs):
        print('开始标签：<%s>' % tag)

    def handle_endtag(self, tag):
        print('结束标签：</%s>' % tag)

    def handle_startendtag(self, tag, attrs):
        print('<%s/>' % tag)

    def handle_data(self, data):
        print('内容：',data)

    def handle_comment(self, data):
        print('注释信息：<!--', data, '-->')

    def handle_entityref(self, name):
        print('&%s;' % name)

    def handle_charref(self, name):
        print('&#%s;' % name)

parser = MyHTMLParser()
parser.feed('''<html>
<head></head>
<body>
<!-- test html parser -->
    <p>Some <a href=\"#\">html</a> HTML&nbsp;tutorial...<br>END</p>
</body></html>''')

```

12. Pillow：一个操纵图像的库

```python
from PIL import Image, ImageFilter

# 操纵图像：https://pillow-zh-cn.readthedocs.io/zh_CN/latest/handbook/tutorial.html
# 打开图片
im = Image.open('/Users/byron/Desktop/123.jpeg')
# 获取图片的宽，高
w, h = im.size
print('原始图片的宽：%s，高：%s' %(w, h))
# 缩放 50%
im.thumbnail((w // 2, h // 2))
ww, hh = im.size
print('缩放后图片的宽：%s，高：%s' %(ww, hh))
# 将图片进行模糊滤镜
im2 = im.filter(ImageFilter.BLUR)
# 把缩放后的图像用jpeg格式保存，默认会覆盖之前的
im2.save('/Users/byron/Desktop/thumbnail.jpeg', 'jpeg')
# 展示图像
im.show()


# 绘图方式
from PIL import Image, ImageDraw, ImageFilter, ImageFont
import random

# 绘图方式
# 随机字母
def randomFont():
    # random.randint(65, 90)：生成65 - 90 随机一个数字
    # chr 将编码转换为对应的字符
    return chr(random.randint(65, 90))

# 随机颜色
def randomColor():
    return (random.randint(56, 255), random.randint(56, 255), random.randint(56, 255))

def randomColor2():
    return (random.randint(32, 127), random.randint(32, 127), random.randint(32, 127))

# 设置宽高
width = 60 * 4
height = 60
img = Image.new('RGB', (width, height), (255, 255, 255))
# 创建font对象
font = ImageFont.truetype('/Users/byron/Desktop/HYTianZhenTi.ttf', 36)
# 创建Draw对象:
draw = ImageDraw.Draw(img)
# 填充每个像素:
for x in range(width):
    for y in range(height):
        draw.point((x, y), fill=randomColor())
# 输出文字:
for t in range(4):
    draw.text((60 * t + 10, 10), randomFont(), font=font, fill=randomColor2())
# 模糊:
image = img.filter(ImageFilter.BLUR)
image.save('/Users/byron/Desktop/code.jpg', 'jpeg')

```

13. requests：一个用于网络请求的库

```python
import requests

# GET 请求
r = requests.get('https://google.com')
    # 返回状态码
print(r.status_code)
    # 返回response内容
print(r.text)


    # 添加参数，传入一个dict作为params参数
r = requests.get('https://www.douban.com/search', params={'q': 'python', 'cat': '1001'})
#     # 请求地址
print(r.url) # https://www.douban.com/search?q=python&cat=1001
    # 检查编码
print(r.encoding)

# POST 请求
r = requests.post('https://accounts.douban.com/login', data={'form_email': 'abc@example.com', 'form_password': '123456'})
    # requests默认使用application/x-www-form-urlencoded对POST数据编码。如果要传递JSON数据，可以直接传入json参数
params = {'key': 'value'}
r = requests.post('https://accounts.douban.com/login', json=params) # 内部自动序列化为JSON
    # 读取文件
upload_files = {'file': open('report.xls', 'rb')}
cs = {'token': '12345', 'status': 'working'}
r = requests.post('https://accounts.douban.com/login', files=upload_files, cookies=cs, timeout=2.5) # 2.5秒后超时

    # 获取响应头
print(r.headers)
    # 获取cookie
print(r.cookies['ts'])

```

14. chardet：检测编码格式

```python
import chardet

# encoding：当前编码格式，confidence：当前检测概率，1.0 表示100%，language：代表当前语言
print(chardet.detect(b'Hello, world!')) # {'encoding': 'ascii', 'confidence': 1.0, 'language': ''}
data = '离离原上草，一岁一枯荣'.encode('gbk')
print(chardet.detect(data)) # {'encoding': 'GB2312', 'confidence': 0.7407407407407407, 'language': 'Chinese'}
data1 = b'\xc0\xeb\xc0\xeb\xd4\xad\xc9\xcf\xb2\xdd\xa3\xac\xd2\xbb\xcb\xea\xd2\xbb\xbf\xdd\xc8\xd9'
print(chardet.detect(data1)) # {'encoding': 'GB2312', 'confidence': 0.7407407407407407, 'language': 'Chinese'}

```

15. psutil：支持跨平台获取系统信息
https://github.com/giampaolo/psutil

```python
import psutil

# CPU逻辑数量
print(psutil.cpu_count()) # 8
# CPU物理核心
print(psutil.cpu_count(logical=False)) # 4
# 统计CPU的用户／系统／空闲时间
print(psutil.cpu_times()) # scputimes(user=130702.38, nice=0.0, system=103135.14, idle=969344.99)

for item in range(10):
    # cpu 使用频率
    print(psutil.cpu_percent(interval=1, percpu=True))

# 获取 ROM
print(psutil.virtual_memory()) # svmem(total=17179869184, available=5142548480, percent=70.1, used=8856158208, free=124866560, active=5020307456, inactive=4997808128, wired=3835850752)
# 获取 RAM
print(psutil.swap_memory()) # sswap(total=4294967296, used=3976986624, free=317980672, percent=92.6, sin=52030672896, sout=2285604864)

# 获取磁盘信息
print(psutil.disk_partitions())
# 获取磁盘使用情况
print(psutil.disk_usage('/')) # sdiskusage(total=499963174912, used=260722843648, free=239240331264, percent=52.1)
# 磁盘IO
print(psutil.disk_io_counters()) # sdiskio(read_count=9349583, write_count=7125064, read_bytes=196497158144, write_bytes=179151810560, read_time=6873642, write_time=2788507)

# 获取网络读写字节／包的个数
print(psutil.net_io_counters()) # snetio(bytes_sent=6174398464, bytes_recv=6251361280, packets_sent=8622716, packets_recv=17914967, errin=0, errout=16921, dropin=0, dropout=0)
# 获取网络接口信息
print(psutil.net_if_addrs())
# 获取网络接口状态
print(psutil.net_if_stats())
# 获取当前网络连接信息
print(psutil.net_connections())

# 获取进程信息
print(psutil.pids())

# 获取指定进行
p = psutil.Process(3776)
# 进程名称
print(p.name()) # akd
# 进程路径
print(p.exe()) # ...
# 进程工作目录
print(p.cwd())
...

```


## virtualenv
* virtualenv就是用来为一个应用创建一套“隔离”的Python运行环境


## 网络编程

1. TCP编程：TCP是建立可靠连接，并且通信双方都可以以流的形式发送数据

* 客服端socket
```python
import socket
# 创建一个客户端socket:
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 建立链接
s.connect(('www.fbyron.cn', 80))
# 发送数据:
s.send(b'GET / HTTP/1.1\r\nHost: www.sina.com.cn\r\nConnection: close\r\n\r\n')
# 接收数据:
buffer = []
while True:
    # 每次最多接收1k字节:
    d = s.recv(1024)
    if d:
        buffer.append(d)
    else:
        break
data = b''.join(buffer)

# 关闭连接:
s.close()

header, html = data.split(b'\r\n\r\n', 1)
print(header.decode('utf-8'))
# 将html存入本地
with open('/Users/byron/Desktop/s.html', 'wb') as f:
    f.write(html)

```

* 服务端socket，可以把一下两段代码放到不同文件然后跑起来看看效果
```python
import socket, threading, time
# 创建一个服务端socket:
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 监听端口
s.bind(('127.0.0.1', 9999))
# 设置等待连接的最大数量
s.listen(5)
print('Waiting for connection...')

def tcplink(sock, addr):
    print('Accept new connection from %s:%s...' % addr)
    sock.send(b'Welcome!')
    while True:
        data = sock.recv(1024)
        time.sleep(1)
        if not data or data.decode('utf-8') == 'exit':
            break
        sock.send(('Hello, %s!' % data.decode('utf-8')).encode('utf-8'))
    sock.close()
    print('Connection from %s:%s closed.' % addr)

while True:
    # 接受一个新连接:
    sock, addr = s.accept()
    # 创建新线程来处理TCP连接:
    t = threading.Thread(target=tcplink, args=(sock, addr))
    t.start()

#-------------------------------------------

import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 建立连接:
s.connect(('127.0.0.1', 9999))
# 接收欢迎消息:
print(s.recv(1024).decode('utf-8'))
for data in [b'Michael', b'Tracy', b'Sarah']:
    # 发送数据:
    s.send(data)
    print(s.recv(1024).decode('utf-8'))
s.send(b'exit')
s.close()

```

2. UDP编程：UDP是面向无连接的协议

```python
import socket
# 服务端
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 绑定端口:
s.bind(('127.0.0.1', 9999))
print('Bind UDP on 9999...')
while True:
    # 接收数据:
    data, addr = s.recvfrom(1024)
    print('Received from %s:%s.' % addr)
    # 向客户端发送数据
    s.sendto(b'Hello, %s!' % data, addr)

# --------------------------------------------

import socket
# 客户端
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
for data in [b'Michael', b'Tracy', b'Sarah']:
    # 发送数据:
    s.sendto(data, ('127.0.0.1', 9999))
    # 接收数据:
    print(s.recv(1024).decode('utf-8'))
s.close()

```


## Web开发

1. WSGI接口：Web Server Gateway Interface

```python
# 从wsgiref模块导入:
from wsgiref.simple_server import make_server

def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    body = '<h1>Hello, %s!</h1>' % (environ['PATH_INFO'][1:] or 'web')
    return [body.encode('utf-8')]

# 创建一个服务器，IP地址为空，端口是8000，处理函数是application:
httpd = make_server('', 8000, application)
print('Serving HTTP on port 8000...')
# 开始监听HTTP请求:
httpd.serve_forever()

```

2. flask

```python
from flask import Flask
from flask import request

app = Flask(__name__)
@app.route('/', methods=['GET', 'POST'])
def home():
    return '<h1>Home</h1>'

@app.route('/signin', methods=['GET'])
def signin_form():
    return '''<form action="/signin" method="post">
              <p><input name="username"></p>
              <p><input name="password" type="password"></p>
              <p><button type="submit">Sign In</button></p>
              </form>'''

@app.route('/signin', methods=['POST'])
def signin():
    # 需要从request对象读取表单内容：
    if request.form['username']=='admin' and request.form['password']=='password':
        return '<h3>Hello, admin!</h3>'
    return '<h3>Bad username or password.</h3>'

if __name__ == '__main__':
    app.run()

```


## 异步IO

1. 协程/微线程：一般来说子程序是顺序调用的，A -> B -> C -> B -> A 完成一次调用，但是协程执行中的子程序是可以中断的，然后转而执行别的子程序，在适当的时候再返回来接着执行，有点类似于线程的切换，但是要比线程快很多，因为子程序不是线程切换，而是程序自身控。

```python
# Python对协程的支持是通过generator实现的
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def product(c):
    # 这里相当于启动生成器，调用send（None）完全等同于调用生成器的next（）方法
    c.send(None)
    n = 0
    while n < 3:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
product(c)

```

2. asyncio：实现单线程并发IO操作

```python
# asyncio
import asyncio

async def hello():
    print('Hello')
    r = await asyncio.sleep(1)
    print("Hello again!")

loop = asyncio.get_event_loop()
loop.run_until_complete(hello())
loop.close()

```

3. aiohttp：asyncio实现了TCP、UDP、SSL等协议，aiohttp则是基于asyncio实现的HTTP框架

```python
import asyncio
from aiohttp import web

async def index(request):
    await asyncio.sleep(0.5)
    return web.Response(body=b'<h1>Index</h1>', content_type='text/html')


async def home(request):
    await asyncio.sleep(0.5)
    text = '<h1>hello %s<h1>' % request.match_info['name']
    return web.Response(body=text.encode('utf-8'), content_type='text/html')


async def init(loop):
    app = web.Application(loop=loop)
    app.router.add_route('GET', '/', index)
    app.router.add_route('GET', '/home/{name}', home)
    srv = await loop.create_server(app.make_handler(), '127.0.0.1', '9998')
    print('Server started at http://127.0.0.1:9998...')
    return srv


loop = asyncio.get_event_loop()
loop.run_until_complete(init(loop))
loop.run_forever()

```


## virtualenv
* 目的是为了分离环境，让不同项目都在自己虚拟环境内安装包等

```shell
# 安装
pip3 install virtualenv

# 在项目目录下，然后就会发现项目目录生成了venv文件
virtualenv venv
||
python3 -m venv ./venv

# mac 下使用source进入当前环境
source venv/bin/activate
# 执行上述命令会发现当前控制台为 (venv) ➜  pythonWebApp，这样我们安装就会在当前的项目中了

# 退出当前环境到全局python环境
deactivate

```


## FastApi

### 1. 安装

```shell
# 安装 fastapi
pip install fastapi

# 安装ASGI 服务器
pip install uvicorn

# main.py
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}

# 启动服务，热更新
uvicorn main:app --reload

# 访问 http://127.0.0.1:8000/
{
    "Hello": "World"
}

```

### 2. 路径参数

```python
# 定义一个路径参数
@app.get("/item/{item_id}")
async def get_item(item_id):
    return {"message": "Hello World", "item_id": item_id}
# curl http://127.0.0.1:8000/item/123   {"message":"Hello World","item_id":"123"}


# 定义路径参数类型
@app.get("/itemint/{item_id}")
async def get_item(item_id: int):
    return {"message": "Hello World", "item_id": item_id}
# curl http://127.0.0.1:8000/itemint/999    {"message":"Hello World","item_id":999}

# curl http://127.0.0.1:8000/itemint/qwe    {"detail":[{"loc":["path","item_id"],"msg":"value is not a valid integer","type":"type_error.integer"}]}

```

* Pydantic：所有的数据校验都是由Pydantic来完成的

* 路径设置很重要，比如 /users/me 就要放在 /users/{name} 之前，不然后者就会把前者进行匹配，前者就接受不到请求了

* 预设值：可以理解为使用枚举

```python
# 定义枚举
class EnumModel(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

@app.get("/models/{model_name}")
async def read_model(model_name: EnumModel):
    if model_name == EnumModel.alexnet:
        return {"model_name": model_name, "message": "this is alexnet"}
    if model_name == "resnet":
        return {"model_name": model_name, "message": "this is resnet"}
    return {"model_name": model_name, "message": "this is author"}

# curl http://127.0.0.1:8000/models/alexnet  {"model_name":"alexnet","message":"this is alexnet"}

```

* 包含路径的路径参数：可以理解为直接传递一个地址，并接收到

```python
@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    return {"file_path": file_path}
# curl http://127.0.0.1:8000/files//home/johndoe/myfile.txt  {"file_path":"/home/johndoe/myfile.txt"}
# curl http://127.0.0.1:8000/files/123/123/123  {"file_path":"123/123/123"}

# 如果不设置 path 的话
@app.get("/files/{file_path}")
async def read_file(file_path: str):
    return {"file_path": file_path}
# curl http://127.0.0.1:8000/files//home/johndoe/myfile.txt {"detail":"Not Found"}
# curl http://127.0.0.1:8000/files/homr   {"file_path":"homr"}

```


### 3. 查询参数

* 声明不属于路径参数的其他函数参数时，它们将被自动解释为"查询字符串"参数，（说人话就是url问号后面的参数解析）

```python
# mock数据
fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]
# 可以理解为是分页把
@app.get('/items/')
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip: skip + limit]

# curl http://127.0.0.1:8000/items/\?skip\=0\&limit\=2  [{"item_name":"Foo"},{"item_name":"Bar"}]

```

* 可选参数

```python
from typing import Union
# 如果你要设置一个必须的查询参数，那么就不用给该参数设置默认值就好，比如参数 s
# q: Union[int, None] = None 这个的意思是 q 可以是 str 或者 None，默认为None
@app.get("/items/{item_id}/")
async def get_item(item_id: str, s:int, q: Union[str, None] = None):
    result = {"item_id": item_id, "s": s}
    print(q, 'q')
    if q:
        result.update({"q": q})
    return result

# curl http://127.0.0.1:8000/items/1/\?q\=abc\&s\=1    {"item_id":"1","s":1,"q":"abc"}

```


### 4. 请求体

* request

```python
from pydantic import BaseModel
# 请求体
class Model(BaseModel):
    name: str
    description: Union[str, None] = None
    price: float
    tax: Union[float, None] = None

@app.post('/items/')
async def get_items(item: Model):
    item_dict = item.dict()
    # 可以直接获取 model 的数据
    print(item_dict, 'item_dict') # {'name': 'byron', 'description': None, 'price': 123.45, 'tax': None} item_dict
    return item

# curl -d '{"name": "byron", "price": 123.45}' -H "Content-Type: application/json" -X POST http://127.0.0.1:8000/items/     {"name":"byron","description":null,"price":123.45,"tax":null}

```


### 5. 查询参数和字符串校验

```python
from fastapi import FastAPI, Query
# 使用Query 可以设置一些校验，max_length：最大长度，min_length：最小长度，regex：正则匹配
@app.get('/itemsq/')
async def get_itemsq(q: Union[str, None] = Query(default=None, max_length=50, min_length=3, regex="^fixedquery")):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

# curl http://127.0.0.1:8000/itemsq/\?q\=fixedquery     {"items":[{"item_id":"Foo"},{"item_id":"Bar"}],"q":"fixedquery"}

# 声明为必需参数
@app.get('/itemsq/')
async def get_itemsq(q: str = Query(max_length=50, min_length=3, regex="^fixedquery")):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results


# 查询参数列表 / 多个值
@app.get('/itemsq/')
async def get_itemsq(q: Union[List[str], None] = Query(default=None)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

# curl http://127.0.0.1:8000/itemsq/\?q\=fixedquery\&q\=123\&q\=qwe     {"items":[{"item_id":"Foo"},{"item_id":"Bar"}],"q":["fixedquery","123","qwe"]}


# 别名参数
@app.get('/itemsq/')
async def get_itemsq(item_query: Union[List[str], None] = Query(default=None, alias="item-query")):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if item_query:
        results.update({"item_query": item_query})
    return results
# 注意这里url的 item-query 参数
# curl http://127.0.0.1:8000/itemsq/\?item-query\=qwe   {"items":[{"item_id":"Foo"},{"item_id":"Bar"}],"item_query":["qwe"]}


```

* 还有一些针对文档的变量如 title：<span style="color: red">应该是标题</span>， description：详情内容，deprecated：是否弃用



### 6. 路径参数的数值校验

```python
from fastapi import FastAPI, Path
# 路径参数和数值校验
@app.get("/items/{item_id}/{path_id}")
async def read_items(
    # ge=0：大于0，le=99：小于99
    item_id: int = Path(title="The ID of the item to get", ge=0, le=99),
    # gt=1.5：大于1.5， lt=9.9：小于9.9
    path_id: float = Path(title='this is path_id', gt=1.5, lt=9.9),
    q: Union[str, None] = Query(default=None, alias="item-query"),
):
    results = {"item_id": item_id, "path_id": path_id}
    if q:
        results.update({"q": q})
    return results

# curl http://127.0.0.1:8000/items/99/9.8   {"item_id":99,"path_id":9.8}


```













## 爬虫
* 请求数据，获取数据，解析数据，展示数据
