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
```