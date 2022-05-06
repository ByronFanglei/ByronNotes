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


## 基础语法

1. 基本命令

```python
# 输入
name = input()
# 输出
print(name)

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

# 格式化
'Hello, %s' % 'world'
'Hello, world'

'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'

'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'

r = 2.5
s = 3.14 * r ** 2
print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62

```

2. list，tuple

```python
# list 类似js数组
classname = [1,2,3,4,5]
len(classname)
5

# 尾部添加元素
classname.append(111) # [1, 2, 3, 4, 5, 111]
# 指定位置添加元素
classmates.insert(1, 'Jack') # ['Jack', 1, 2, 3, 4, 5, 111]
# 删除尾部元素 / 删除指定位置元素，返回删除的元素
classname.pop()
classname.pop(1)

# tuple 元组 tuple和list非常类似，但是tuple一旦初始化就不能修改
classmates = ('Michael', 'Bob', 'Tracy')
# 当定一个只有一个元素的元祖 需要这样设置
classmates = ('Michael',)

list(range(5)) # range 生成一个指定整数序列
[0, 1, 2, 3, 4]
```

3. 条件判断

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


4. 循环

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


4. dict，set

* dict和list比较，dict有以下几个特点：

	1. 查找和插入的速度极快，不会随着key的增加而变慢；
	2. 需要占用大量的内存，内存浪费多。

* 而list相反：

	1. 查找和插入的时间随着元素的增加而增加；
	2. 占用空间小，浪费内存很少。

```python
# dict 类似于 js 的Map key value形式的存储数据，或者说js只是模仿者吧
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
d['Michael']
95

# 添加
d['Adam'] = 67

# 判断 key 是否存在
'Bob' in d
True
'Boba' in d
False

# 通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value
d.get('Thomas')
d.get('Thomas', '么有')
'么有'

# 删除某个值
d = {'Michael': 95, 'Tracy': 85, 'Bob': 75}
d.pop('Bob')
75
d
{'Michael': 95, 'Tracy': 85}



# set类似于 js 的Set，重复元素会被自动过滤
s = set([1, 2, 3])	# 写用的 []
s
{1, 2, 3}		# 查看时 {}

# 添加元素
a.add(111)

# 删除元素
a.remove(111)

s1 = set([1, 2, 3])
s2 = set([2, 3, 4])
# 两个元素的交集
s1 & s2
{2, 3}
# 两个元素的并集
s1 | s2
{1, 2, 3, 4}
```


## 函数部分

1. 常用函数

```python
# abs 绝对值
abs(100)
100
abs(-20)
20
abs(12.34)
12.34

# 最大最小
max(2, 3, 1, -5)
3
min(2, 3, 1, -5)
-5

# 数据类型转换
int('123')
123
int(12.34)
12
float('12.34')
12.34
str(1.23)
'1.23'
str(100)
'100'
bool(1)
True
bool('')
False
```


2. 定义函数

```python
# def 定义函数
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
        
# 空函数
def nop():
    pass

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