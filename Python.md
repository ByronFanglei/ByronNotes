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
# 参数前加 *会使参数numbers接收到的是一个tuple，可以立即成 js 的arguments
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
    
# 打印list 或者 tuple 加* 可以理解为 js 的解构
nums = [1, 2, 3, 4, 5]
print(*nums, nums)
# 1 2 3 4 5 [1, 2, 3, 4, 5]


# 关键字参数
# **extra表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('byron', 18, **extra)
# name: byron age: 18 other: {'city': 'Beijing', 'job': 'Engineer'}


# 命名关键字参数，* 号后面的是必须要传递的
def person(name, age, *, city, job):
    print(name, age, city, job)
person('Jack', 24, city='Beijing', job='Engineer')


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
# 切片 [0:3] 意思就是 从0开始截取3个，当然如果是0开始的话可以不用写0 直接[:3], 当然也支持负数
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
print(L[1:3], L[-2:], L[-2:-1])
# ['Sarah', 'Tracy'] ['Bob', 'Jack'] ['Bob']


# [0::5] 表示 从0开始每5个取一次，如果是0开始的话，这个0可以不写
# 甚至可以这样 L[:]，直接复制当前的 list
L = list(range(20))
print(L[::5], L[10::5])
[0, 5, 10, 15] [10, 15]

# 字符串也可以这么搞
'ABCDEFG'[:3]
'ABC'
'ABCDEFG'[::2]
'ACEG'

# 如果要打印list的下标可以使用enumerate包装一下
for i, value in enumerate(['A', 'B', 'C']):
     print(i, value)
     
     
# range 
# list(range(1, 10, 3), 表示生成一个从 1 开始 10 结束的list，每个 3 个取一个值
[1, 4, 7]

# 一行代码生成指定的list，这里可以看到 for 前的 if 是有 else 的 for 后的 if 是没有 else 且是不可以加的，是因为之前的 if else 是一个 表达式，之后的 if 是一个过滤条件 
print([x * x for x in range(1, 10) if x % 2 == 0])
[4, 16, 36, 64]
print([x * x if x % 2 == 0 else -x * x for x in range(1, 10)])
[-1, 4, -9, 16, -25, 36, -49, 64, -81]

# Iterable 类型包括：list、tuple、dict、set、str，generator
from collections.abc import Iterable
print(isinstance([], Iterable))
True
print(isinstance({}, Iterable))
True
print(isinstance('abc', Iterable))
True
print(isinstance((x for x in range(10)), Iterable))
True
print(isinstance(100, Iterable))
False


# Iterator：可以被next()函数调用并不断返回下一个值的对象称为迭代器
from collections.abc import Iterator
isinstance((x for x in range(10)), Iterator)
True
isinstance([], Iterator)
False
isinstance({}, Iterator)
False
isinstance('abc', Iterator)
False

# 生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iterator。
# 把list、dict、str等Iterable变成Iterator可以使用iter()函数：
isinstance(iter([]), Iterator)
True
isinstance(iter('abc'), Iterator)
True

# 凡是可作用于for循环的对象都是Iterable类型
# 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列
```


## 函数式编程

1. 高阶函数

```python
# map 传入的第一个参数是f，即函数对象本身。由于结果r是一个Iterator，Iterator是惰性序列，因此通过list()函数让它把整个序列都计算出来并返回一个list
def f(x):
    return x * x

r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
print(list(r))
[1, 4, 9, 16, 25, 36, 49, 64, 81]

# reduce reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算
from functools import reduce
def fn(x, y):
	return x * 10 + y

reduce(fn, [1, 3, 5, 7, 9])
13579

# filter filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
[1, 5, 9, 15]

# sorted 排序算法
sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
# sorted第二个参数可以传递一个key，可以对排序的内容进行处理后再次排序
sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
# 如果需要进行反向排序，传递第三个参数即可
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```