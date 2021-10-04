---
layout:     post
title:   Effective C++(16-17) new,delete,智能指针
subtitle:   Effective C++(16-17) new,delete,智能指针
date:       2021-10-04
author:     LXR
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Effective C++
    - new,delete,智能指针
---

# 参考文章
书：《Effective C++》

# 请记住
成对使用new,delete要用配套形式。  
以独立语句将new的对象置于智能指针，否则可能导致由异常引发的难以察觉的资源泄露。  
# new,delete
> * new + delete
> * new [] + delete []
> * 不对数组做typedef

形式不配套可能导致额外的内存被释放，或者内存释放不完全。  

# 智能指针
```
int prority();
void processWidget(std::tr1::shared_ptr<Widget> pw, int prority);

// 用户代码
processWidget(std::tr1::shared_ptr<Widget>(new Widget()), prority());
```

编译器可能的创建代码的顺序为:
> * 1. new Widget(); 
> * 2. prority(); 
> * 3. shared_ptr构造函数;

万一prority()发生异常，new Widget返回的指针将会遗失，导致资源泄露。  

解决办法，独立语句：
```
std::tr1::shared_ptr<Widget> pw(new Widget());

processWidget(pw, prority());
```

