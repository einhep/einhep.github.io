---
layout: post
title: "day1-algrithms-递归算法"
subtitle: "利用递归算法解决汉诺塔问题、阶乘问题和斐波那契数列问题"
data: 2018-08-13 21:26:00
author: "einhep"
header-img: "img/post-bg-2015"
tags:
    - 100-days-of-challenge
    - 算法
    - 机器学习
---
# 递归算法
## 汉诺塔问题
[示意图](./img/in-post/hanoi.png)
一般递归思路能解决的问题是能够分解成为重复的模式，例如在汉诺塔问题中，如果我们正向考虑问题，那么久会陷入困境。但是如果我们反向考虑问题那么事情就会出现转机。
如上图所示，我们倒着考虑问题，解决N个盘子移动的思路分为3步，第一步把N-1个盘子放到b上面，第二步把a上面剩下的盘子放到c上面，第三步把b上面的N-1个盘子放到c上面。从这个角度考虑问题的时候，我们发现第一步和第三步又是一个hannoi的问题 所以问题的解决就得到了简化。
       
    def hanoi(height, left='left', right='right', middle='middle'):
      if height:
        hanoi(height-1, left, middle, right)
        print(left '=>' right)
        hanoi(height-1, middle, right, left)
        
## 阶乘问题
对于阶乘问题，我们知道N! = N*(N-1)!.这就是一个明显的递归解决的例子。下面我们进行分部解决。第一步求出N-1的阶乘，第二步将其诚意N并返回结果

    def jiecheng(num):
      if num == 0:
        return 1
      else:
        return num*jiecheng(num-1)
        

## 斐波那契数列问题
著名的斐波那契数列定义如下，可以看出，f(n)是由规模更小一些的f(n-1)和f(n-2)推导出来的：

f(0)=0，f(1)=1

f(n)=f(n-1)+f(n-2) (n>=2)

因此，递归实际上就是用自己来定义自己。

我们进行分步，第一步求解n-1的结果，第二步求解n-2的结果，第三步相加得到n的结果

    def fob(num):
      if num == 0:
        return 0
      if num == 1:
        return 1
      return fob(num-1)+fob(num-2)

