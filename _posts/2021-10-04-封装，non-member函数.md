---
layout:     post
title:   Effective C++(22-24) 封装，non-member函数
subtitle:   Effective C++(22-24) 封装，non-member函数
date:       2021-10-04
author:     LXR
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Effective C++
    - 封装，non-member函数
---

# 参考文章
书：《Effective C++》

# 请记住
将成员变量声明为private;protected并不比public更具封装性。  
宁以non-member non-friend函数替换member函数。  
如果需要为某个函数的所有参数进行类型转换，必须是non-member函数。  

# 将成员变量声明为private
将成员变量声明为private，通过成员函数实现“禁止访问”，“只读访问”，“读写访问”等。 
如果将成员变量声明为public或protected，则扩展时太多的代码需要重写。  
从封装的角度看，只有两种访问权限：private（封装）和其他（不封装）。  

# 宁以non-member non-friend函数替换member函数
如果越多函数可以访问数据，则数据的封装性越低。  
non-member non-friend函数并不增加“能够访问class内的private成分”的函数数量。  
将所有便利函数放在多个头文件但隶属于同一个命名空间，每个头文件实现部分机能，方便客户扩展，减少编译依赖。  
```
namespace WebBrowserStuff{
class WebBrowser{
public:
      void clearCache();
      void clearHistory();
      void clearCookies();
      ...
};
}

namespace WebBrowserStuff{
    void clearBrowser(WebBrowser& w)
    {
        w.clearCache();
        w.clearHistory();
        w.clearCookies();
    }
}

namespace WebBrowserStuff{ // "webbrowserbookmarks.h"
    ...
}

namespace WebBrowserStuff{ // "webbrowsercookies.h"
    ...
}
```

# 如果需要为某个函数的所有参数进行类型转换，必须是non-member函数
```
class Rational{
public:
      Rational (int numerator = 0, int denominator = 1); // 允许隐式类型转换
      
      int numerator() const;
      int denominator() const;
      
      const Rational operator*(const Rational& lhs, const Rational& rhs);
private:
       ...
};
//用户代码
Rational oneEight(1, 8);
Rational oneHalf(1, 2);

Rational result;
result = oneHalf * 2; // yes, 2用来构造，构造函数非explicit
result = 2 * oneHalf; // no， 没有int和Rational的operator*接受调用
// oneHalf * 2 等价于 oneHalf.operator*(2)，只有当参数位于参数列内才能进行隐式类型转换
// 2 * oneHalf 等价于 2.operator(oneHalf)，this的隐喻参数不能进行隐式类型转换

const Rational operator*(const Rational& lhs, const Rational& rhs); // non-member函数
result = oneHalf * 2; // yes
result = 2 * oneHalf; // yes
```


