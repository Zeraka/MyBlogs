---
layout: post
title: apply filter map reduce in python
subtitle: python数据处理
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [Python, Data]
comments: true

---

## apply

apply函数是pandas中自带的用于行列处理的函数，除此之外我看到网上有把apply单独拿出来用的文章，但是试验了很多次无法通过。

pandas自带的apply形式与示例为：

```
# apply(function,axis=1 | 0)
# function就是对axis(指定的行或者列)中的每个元素所使用的函数
data=pd.DataFrame(a)
data.apply(lambda x:x*10)
```

apply函数对DataFrame或者Series类型的数据进行操作，会按行或者按列遍历执行放入apply中的函数。

举例说明:

```
import pandas as pd
import numpy as np
import math

def adder(a,b):
	return a+b

d = {'Score_Math':pd.Series([66,57,75,44,31,67,85,33,42,62,51,47]),'Score_Science':pd.Series([89,87,67,55,47,72,76,79,44,92,93,69])}

df = pd.DataFrame(d)
print(df)

# Use apply function
print(df.apply(np.mean,axis=1))
print(df.apply(np.mean,axis=0))

```

输出结果如下：

```
    Score_Math  Score_Science
0           66             89
1           57             87
2           75             67
3           44             55
4           31             47
5           67             72
6           85             76
7           33             79
8           42             44
9           62             92
10          51             93
11          47             69
0     77.5
1     72.0
2     71.0
3     49.5
4     39.0
5     69.5
6     80.5
7     56.0
8     43.0
9     77.0
10    72.0
11    58.0
dtype: float64
Score_Math       55.0
Score_Science    72.5
dtype: float64
```

axis为0时，代表处理列，为1时代表处理行。apply函数遍历每一列或者每一行，执行同样的函数。

apply可以使用pandas或者numpy自带的行列计算的函数，也可以自定义函数。

### apply中的自定义函数

一般和lambda结合使用。

apply自定义function格式为：

```
def function(x[,a,b...]):
	pass
```

除了第一个参数之外，其余的参数均可以自定义，其余的形参在赋实参时需要在apply中具体写出来：

```
df.apply(function, a=1,b=2) 
```

自定义函数中的第一个参数数据类型是Series, 即Dataframe中的一行或一列。你既可以对Series整体进行计算，也可以像列表一样提取出其中的具体的元素。

```
def fn1(x):
	return x**2
def fn2(x):
	return x[1]
```

自定义函数中的参数是一个Series，其实就是一组向量，因而在形式上可以使用向量运算。多个向量运算之后会得到一个新的向量。

对向量进行运算并不需要一定在apply中调用。直接定义形参类型为Series的函数即可。

```
df=pd.DataFrame({'a':[10,20,30],'b':[20,30,40]})

def myAvg(x,y):
	return (x+y)/2
myAvg(df['a'],df['b'])
```

由于函数中的形参都是Series类型，如果想对向量中存在的某一个元素进行判断，就要将函数“向量化”，使得Series类型的数据可以和单个数字进行运算。

```
def myavg_2(x,y):
	if x == 20:
		return (np.NaN)
	else:
		return (x+y)/2
myavg_vec=np.vectorize(myavg_2)
myavg_vec(df['a'],df['b'])

# output
# array([15,nan,35])
```

或者使用装饰器，可以达到同样的效果

```
@np.vectorize
def myavg(x,y):
	if x == 20:
		return (np.NaN)
	else:
		return (x+y)/2
```

[apply自定义函数](https://blog.csdn.net/qq_41341757/article/details/110442681)

### 使用lambda函数

lambda函数就是匿名函数，无须定义函数而使用lambda表达式，使得代码简洁精简。

lambda表达式仅仅是一种定义函数的方法，一方面它使得代码更加精简，另一方面它降低了代码可读性。

```
demo=lambda x:x+n
a=demo(4)
print(a(6))
```

[Python lambda](https://www.w3school.com.cn/python/python_lambda.asp)

apply函数通常与grouby函数一起使用，apply函数的用法就是有一些方案的

[Python中apply函数详解](https://www.jb51.net/article/234702.htm)

[APPLY FUNCTIONS IN PYTHON PANDAS – APPLY(), APPLYMAP(), PIPE()](https://www.datasciencemadesimple.com/apply-functions-python-pandas-apply-applymap-pipe/)

## Reduce函数

Reduce函数在python2中为内置模块，在python3中放到了functools模块，需要pip3安装。使用时需要导入：

```
# reduce(function, iterable)
from functools import reduce
y=[2,3,4,5,6]
reduce(lambda x,y: x+y,y)
```

reduce传入的参数是两个，第一个参数是自定义函数，第二个是列表。自定义的函数也必须是两个参数，代表着列表中的第N个元素和第N+1个元素，其返回值会作为一个新的第N+1个元素，然后与第N+2个元素重新执行该函数，依此执行，最终得到一个值。reduce的含义就是缩小，压缩。

## map函数

map的本意是映射，函数一定是映射，但映射未必是函数。在C++和Java语言中，map被引申为一种key-value的容器。但是python中的map函数，仍代表映射的本意。map函数操作的对象是一个列表或者一组向量。通过map函数，我们无需编写循环语句。

经过map操作过的序列会变成一个新的序列。Python3中的map操作之后返回的是一个迭代器。

## filter函数

filter函数中的自定义函数的返回值一定为真或为假，原有序列只会保留执行自定义函数后结果为真的元素，最终生成一个新序列。

```
x=[1,2,3,4,5]

list(filter(lambda x:x%2==0,x)) # x若为偶数则留下

# output
# [2,4]
```

[python中的filter、map、reduce、apply用法总结](https://blog.csdn.net/m0_67401746/article/details/126482448)

## 比较

map和filter都是python自带的模块，且它们执行得到的结果仍是一个序列，想打印出结果就要用 list(map())和list(filter())的形式，而reduce得到的是一个值，因此可以直接打印。



