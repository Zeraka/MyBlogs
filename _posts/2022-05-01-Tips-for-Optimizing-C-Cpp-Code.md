---
layout: post
title: 优化C/C++代码的建议
subtitle: <Tips for Optimizing C/C++ Code>译注
tags:[C++, Linux, Performan]
---

# 优化C/C++代码性能的27条建议——<Tips for Optimizing C/C++ Code>译注

<font color='gree'>本文来自people.cs.clemson.edu的计算机图形学课程，编号405。关于C++语言，其广泛的应用便是桌面UI和计算机图形学领域。</font>

## 1.记住Ahmdal法则： 

$$
Speedup=\frac {time_{old}}{time_{new}}=\frac {1}{(1-func_{cost})+func_{cost}/func_{speedup}}
$$

$func_{cost}$是函数func被使用时的运行占总程序时间比，$func_{speedup}$是该函数相比原来提升倍率。

假入要优化函数*TriangleIntersect()*, 有40%的运行时，那么将其运行速率提升一倍后，程序总的运行速率变为原来的1.25倍。
$$
\frac 1{(1-0.4)+0.4/2}=1.25
$$


这意味着不经常使用的代码(infrequently used code)应该被很少去优化。

## 2.第一次就编码正确，然后才优化它。 

这并不意味着要用8周写一个功能完备的光线跟踪器，然后再用8周去优化它。

基于正确编写的代码，然后再清楚被经常使用的函数是哪些，再去优化它。

发现瓶颈，去除瓶颈，通过优化或者提升算法。经常提升算法可以明显消除瓶颈，可能效果出乎意料。对于你经常使用的函数，不断进行优化是个好主意。

## 3.我知道的写的非常高效的代码的人，都说他们花费了至少两倍的时间在优化代码上。

<font color="gree">写作也如此，想写出好的文章，必须要多多修改。</font>

## 4.跳转/分支代价高，尽可能不使用它们。

函数调用需要两次跳转，额外操纵内存栈。

枚举胜过递归(依然涉及到内存栈多次操作)。

对于短小函数使用内联函数模式(inline Functions)来降低函数开销。

在函数调用内部，改变循环的写法：

```
把 
	for(i=0;i<100;i++)
	do sth
改成
	do sth
	{
	for(i=0,1<100;1++)
	{……}
	}
	# 以上意思是，执行开销大的动作非必要不要向循环里放，循环中放判断语句。
```

大量的if...else if...else if..else if... 语句在结束整个判断链条前需要大量跳转到这些case(因为要尝试每种情况)。如果可能的话，把它们换成*switch*语句,使得编译器可以将之优化为单一跳转的表循环(a table lookup)。如果无法使用switch语句，就把最容易执行的判断语句放到判断链条之前。

## 5.思考数组参数的顺序。

二维或者更高维的数组仍然存储在一维的内存中。这意味着(对于C/C++数组)`Array[i][j]`和`Array[i][j+1]`相邻(are adjacent to each other)，但是对于`Array[i][j]`和`Array[i+1][j]`来说它们可能相距很远(may be arbitrary far apart)。

更多或者更少地按顺序方式(fashion)访问被存在内存中的数据，能够动态加速你的代码(有时是一个数量级或者更多)。

```
Accessing data in a more-or-less sequential fashion, as stored in physical memory, can dramatically speed up your code(sometimes by an order of magnitude,or more)!
```

当现代CPU从主要内存中加载数据到处理器Cache中时，它们会取出多个值。而它们会取出一块包含被请求的数据和相邻的数据(这一块数据叫cache line).这意味着`a[i][j]`之后数据也会在CPU cache中，`array[i][j+1]`有很大机会已经在cache中，但是`array[i+1][j]`可能仍然只在内存中。

## 6.考虑指令级并行运算

尽管还能多应用仍然依赖单线程执行，现代的CPU已经有明显大量的单核并行计算能力。这意味着单个CPU可以被模拟执行4个浮点运算，同时等待4个内存请求，并对即将到来的分支进行比较。

大部分并行操作，代码块都需要有足够的依赖结构来使得CPU充分优化。

考虑用展开循环(unrolling loops)提升它。

这也是使用inline内联函数的绝好理由。

## 7. 避免本地变量的数量

本地变量一般存储在栈中，然而如果没有足够的栈，它们就会被存在寄存器(register)中, 在这种情况下，函数不仅从存储在寄存器中的数据更快地访问你存，而且函数还避免了设置栈帧(stack frame)的过度开销。

永远不要大量(wholesale, 批量，大量)切换到全局变量！

## 8.减少函数参数的数量

与减少本地变量(local variables,又叫局部变量)的里有一样，它们也被保存在栈中。

## 9.通过引用(reference)而不是值传递结构(Pass structures)

我所知道的是在光线追踪中没有一个案例是用值传递的(甚至像是简单的案例如向量，指针和色彩)。

<font color='gree'>值传递涉及拷贝操作</font>

## 10.如果你不需要一个函数的返回值，那么就不要定义它。

<font color="gree">这里是针对性能来说的，一般情况下，性能越好的程序，可读性越差。</font>

## 11.尽可能尝试避免投射

<font color='gree'>Try to avoid casting where possible，cast是投射的意思，联系下文，应该是指不要进行强制类型转换</font>

整型和浮点型运算通常会操作不同的寄存器，因此强制类型转换需要拷贝操作。

短整型(char 和short)仍然需要使用整个(full-sized)寄存器，他们需要被扩充成(be padded to 衬垫，向里面加东西)32/64位然后在存入内存前再转回原先的尺寸。(然而，这样的花费等于额外的大数据类型的内存开销。)

## 12.在声明C++对象变量的时候要小心

使用初始化而不是声明(assignment)一个(例如Color c(black)比Color c;c=black 要快);

## 13.默认的类构造函数(class constructer)尽可能轻

尤其是经常被操作的简单的常用的类(比如color,vector,point等)。

<font color='gree'>Particularly for simple,frequently used classes(eg., color,vector, point, etc) that are manipulated frequently.</font>

这些默认的构造函数常在你不知道与不愿意的情况下被调用。

使用构造函数初始化列表。(使用Color::color():r(0),g(0),b(0){}而不是Color::Color(){r=g=b=0}。)

##  14. 尽可能使用位偏移操作符(shift operation)>>和<<而不是整型的乘法和除法

## 15.小心使用表循环函数(table-lookup functions)

许多人鼓励在复杂函数中(例如三角函数，trigonometric functions)使用预先计算的值的表。对于光线追踪，这通常是不必要的。内存查找开销是非常(exceedingly)高昂且程度逐步加大的，随着三角函数的冲计算越来越来，从内存中取回值的速度就越快(尤其是当你考虑到trig查找会污染CPU缓存时)。

在其他的实例中，查找表可能非常有用。对于GPU编程，表查找通常比复杂的函数有更好的性能表现。

<font color="gree">trig函数是基于表查找的函数</font>

## 16.对于大多数类，使用操作符+=，-=，*=和/=而不是操作符+，-，和/

简单的操作符需要去创建一个未命名的临时对象。

例如：Vector v= Vector(1,0,0)+Vector(0,1,0)+Vector(0,0,1);创建了五个未命名的临时的Vectors对象：Vector(1,0,0),Vector(0,1,0),Vector(0,0,1),Vector(1,0,0)+Vector(0,1,0),Vector(1,0,0)+Vector(0,1,0)+Vector(1,0,0)。

轻微冗长(verbose)的代码:Vector v(1,0,0); v+= Vector(0,1,0); v+= Vector(0,1,0);只需要创建后面两个临时变量：Vector(0,1,0)和Vector(0,0,1)。

## 17.针对基础数据类型，使用操作符`+,-*和|`而不是`+=,-=,*=和|=`

## 18.延迟声明本地变量

声明对象变量总是包含一个函数调用(对于构造函数来说)。

如果一个变量仅仅有时在被需要，则只在必要时候声明，那么构造函数就会只在变量被使用的时候被调用。

## 19.对于对象obj，使用前缀操作符(++obj)而不是后缀操作符(obj++)

在你的光线跟踪中这可能不会是一个问题。

一个对象的拷贝必须用后缀操作符完成(包括了一个额外的对构造函数和析构函数的调用)，但是前缀操作符不需要一个临时的拷贝。

## 20.小心使用模板(templates)

优化各种示例(instantiation)可能需要不同的办法。

标准模板库已经被优化很好，但是我会避免使用它如果你计划去实现一个可交互的光线追踪(implement an interactive ray tracer)。

为什么？**通过自己实现它，你将会知道它运用的算法，所以你将会知道使用代码的最有效方式。**

更重要的是，我的经验是对STL库进行DEBUG是缓慢的，一般来说这不是一个问题，除非你要使用调试版本进行评测(expect you will be using debug versions for profiling)。你将会发现STL构造函数，迭代器等，使用了超过15%的运行时间，这些事件会使得读取文件输出更加混乱(confusing)。

## 21.避免在计算时执行动态内存分配

动态内存用于储存现场事件，其他的数据不会在执行期间改变。

然而，绝大部分系统的动态内存分配需要使用锁来控制访问allocator(access to allocator)。对于使用动态内存的多线程应用，添加额外处理器会导致减速，因为要等待分配内存。

即使对于单线程应用，在堆上分配内存也比在栈上要开销更大。操作系统需要执行一些计算来寻找所需(requisite)尺寸的内存块。

## 22.找到与使用你的系统内存缓存的信息

如果一个数据结构适应单个cache line, 则著需要从主存提取一次数据即可处理整个类。`If a data structure fits in a single cache line, only a single fetch from main memory is required to process the entire class.`

确保所有数据结构被排列(be aligned to )到cache line 边界。(如果你的数据结构和cache line都是128位，你的数据结构的1位会占用1个cache line，另外的127位会占用另一个cache line,这会导致更差的性能。)

## 23.避免不必要的数据初始化

如果初始化一大块内存，考虑使用memset()。

## 24.尝试早点循环终端和早点函数返回

**考虑到光线和三角形(triangle)相交，普遍的例子是，光线将会错过三角形，于是这里应该被优化。**

如果你用三角形横断光线，那么你可以立即返回 光线和平面相交是否为负值，这允许你跳过重心坐标计算(barycentric coordinate computation)。这是重大胜利！一旦你知道没有交叉点发生，交叉点函数就会退出。

<font color=gree>t value是真值的意思，negetive在这里表示负数，intersect 横断，但是翻译为相交更好</font>

## 25.简化在纸上的等式

**许多的等式中，terms被删除了，尽在一些特殊的案例中。**

编译器无法找到这些简化方案，但是你能。在内循环中减少一些开销大的操作能够加速你的程序。

## 26.整型数，固定点，32位float类型，64位double类型的区别没有你想象的大

在现代CPU中，浮点操作一定与整形操作有相同的吞吐。计算密集型的程序像执行光线追踪 ，这会导致整型与浮点型运算的花费有着可以忽略的差别(this lead to a negligable difference between integer and floating-point costs.) 这意味着，你不应该特意(go out of one's way)使用整形运算。

Double精确(precision)浮点操作可能不会比单精度浮点操作慢，尤其是在64位机器上。我已经看见光线追踪使用全double类型比在相同的机器上使用全float要快。

## 27. 思考重新组织数学计算来减少高开销操作

sqrt()常常被避免使用，尤其是比较平方值时，比较结果是相同的。

如果你反复除以x(repeatedly divide by x),可以考虑计算1/x并且乘以结果。这曾经是向量规范化(vector normalizations)的一大胜利,不过我最近发现它现在是一个折腾(toss-up)。 然而如果你做超过3次出发，它就仍然是有用的。

 如果你操作一个循环，确保不会再枚举之间改变的计算从循环中退出(are pulled out the loop)。

考虑到你是否可以在一个循环中增量地(incrementally)计算值(而不是从头开始对每次迭代进行计算)。

[Tips for Optimizing C/C++ Code](https://people.cs.clemson.edu/~dhouse/courses/405/papers/optimize.pdf)

[Markdown数学公式教程](https://www.jianshu.com/p/25f0139637b7)

