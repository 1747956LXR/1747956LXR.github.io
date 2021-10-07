---
layout:     post
title:   Effective C++(2) 以const, enum, inline替换#define
subtitle:   Effective C++(2) 以const, enum, inline替换#define
date:       2021-10-01
author:     LXR
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Effective C++
---

# 参考文章
书：《Effective C++》

# 请记住
对于单纯常量，以const, enum来替换#define。 
对于函数宏，以inline函数替换#define。
其他地方，#define仍有用。 

# const
## const保证变量进入记号表
```
#define RATIO 1.653
// 该记号由预处理器处理，编译器有可能看不见，因而未进入记号表，带来错误

const double Ratio = 1.653;
// 语言常量肯定会被编译器看见，进入记号表
```
## const可以创建class作用域内的对象
```
class A{
private:
       static const double cc; // 声明
};
const A::cc = 1.35; // 实现
```

# enum
class在编译期间需要知道一个class常量值。 
可以取const的地址，而enum和#define不行。 
```
class A{
private:
       enum { NUM = 5 };
       int scores[NUM];
};
```

# inline
```
#define MAX(a,b) f((a) > (b) ? (a) : (b))
// 要为所有实参加小括号，即使如此也难免出错

template<typename T>
inline void max(const T& a, const T& b)
{
    f(a > b ? a : b);
}
// 行为可预料， 类型安全
```
