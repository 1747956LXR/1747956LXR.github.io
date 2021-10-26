---
layout:     post
title:   Effective C++(32) public继承的is-a关系
subtitle:   Effective C++(32) public继承的is-a关系
date:       2021-10-26
author:     LXR
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Effective C++
---

# 参考文章
书：《Effective C++》

# 请记住
public继承表示“is-a”的关系：每一个derived class都是一个base class，适用于base class的每一件事，是的每一件事，也一定适用于derived class。  

# 例子：人和学生
```
class Person {...};
class Student: public Person {...};

void eat(const Person& p);
void study(const Student& s);
Person p;
Student s;
eat(p);
eat(s);
study(s);
// 类型为Person的实参也愿意接受Student对象
```

# 例子：鸟和企鹅
## 乱流
```
class Bird {
public:
    virtual void fly();
    ...
};

class Penguin: public Bird {...};
// 企鹅不会飞
```

## 解决1：新的继承关系
```
class Bird {...};

class FlyingBird: public Bird {
public:
    virtual void fly();
    ...
};

class Penguin: public Bird {...};
```

## 解决2：运行期错误
```
void error(const std::string& msg);
class Penguin: public Bird{
public:
    virtual void fly() { error("Can't fly!");}
    ...
};
```
## 解决3：编译期禁止
```
class Bird {...};

class Penguin: public Bird {...};
```

# 矩形和正方形
```
class Rectangle{
public:
    virtual void setHeight(int newHeight);
    virtual void setWidth(int newWidth);
    virtual int height() const;
    virtual int width() const;
    ...
};

void makeBigger(Rectangle& r)
{
    int oldHeight = r.height();
    r.setWidth(ri.width() + 10);
    assert(r.height() == oldHeight); // 永远为真
}

class Square: public Rectangle {...};
Square s;
...
assert(s.width() == s.height()); // 永远为真
makeBigger(s);
assert(s.width() == s.height()); // 应该为真，但为假
// 可以对矩形单独改变宽度，对正方形不可以
```
