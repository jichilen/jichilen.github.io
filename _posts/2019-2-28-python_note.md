---
layout: post
title:  "python note"
date:   2019-2-28
desc: "something about python decorator"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,learning,decorator]
icon: icon-html
---

##### 写一个带形参的装饰器

装饰器的本身就是一个参数为函数或者类的函数或者类

```python
def INFO(level):
    def debug(fun):
        def wraper(*args,**kwargs):
            print(level+': '+fun.__name__)
            return fun(*args,**kwargs)
        return wraper
    return debug

#some code for testing
class A(object):
    @INFO('debug')
    def print_a(self):
        print("this is fun_a")
```

可以看到在忽略参数的时候 `def debug(fun)`就是一个单纯的函数，得益于python的语法糖，装饰器可以用一种很方便的`@debug`方式来呈现出来，但是本质上就是`print_a=debug(print_a)`

使用类作为装饰器，使用类作为装饰器的时候，上述的语法依旧继承，所以相当于`debug(print_a)`是在做初始化，后面则是调用，由此可见为了能调用，我们需要复写`__call__`方法

```python
class debug(object):
    def __init__(self,fun):
        self.fun=fun
    def __call__(self,*args,**kwargs):
        print(self)
        print('debug: '+self.fun.__name__)
        return self.fun(*args,**kwargs)
@debug
def print_a():
    print("what")
    
#output
#<__main__.debug object at 0x00000000047B0CC0>
#debug: print_a
#what

#不过这种基于类的装饰器有一个问题，就是实例的传入问题
class A(object):
    a=1
    @debug
    def print_a(self):
        print(self.a)
#这个例子可以看到 print_a=debug(print_a)之后 相当于把一个debug的实例强制传入debug类中
#这个时候的print_a(self)变成了print_a(self(debug),self)
#新建实例a=A()，a.print_a()相当于执行了 print_a(a)与前面相比少了一个参数

#a.print_a(a)
#output
#<__main__.debug object at 0x00000000047D75F8>
#debug: print_a
#1
```

可以看到，用类来定义装饰器涉及到实例的传递过程，容易出错

##### 修饰类的装饰器

```python
def Add_A(obj):
    obj.num=1
    return obj

@Add_A
class A():
    c=2

print(A.__dict__)#{'__module__': '__main__', 'num': 1, 'c': 2, '__doc__': None}
```

可以看到修饰类的装饰器更为简单，这是因为类是不带参数的

##### python自带装饰器的理解

python有三种常见的装饰器`@property @staticmethod @classmethod`

`@property`来源于封装的思想，对于一个类，如果所有的对象都是能直接访问的

```
class C(object):
    @property
    def x(self):
        "I am the 'x' property."
        return self._x
    @x.setter
    def x(self, value):
        self._x = value
    @x.deleter
    def x(self):
        del self._x
```



##### 用装饰器实现单例模式

首先是python里面的单例模式，要实现单例，就要在类中建立好对象并且保存在私有变量中，每当新建实例的时候就返回已经新建的类

```python
import threading
class Singleton(object):
    _instance_lock = threading.Lock()
    def __init__(self):
        pass
    def __new__(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            with Singleton._instance_lock:
                if not hasattr(Singleton, "_instance"):
                    Singleton._instance = object.__new__(cls)  
        return Singleton._instance
```

上面的方法是用`__new__()`来实现单例的代码，使用的时候只需要继承该类

下面贴用装饰器实现单例的例子

```python
import threading
from functools import wraps
def singleton(cls):
    _instance={}
    cls._instance_lock=threading.Lock()
    @wraps(cls)
    def wrapper(*args,**kwargs):
        if cls not in _instance:
            with cls._instance_lock:
                if cls not in _instance:
                    _instance[cls]=cls(*args,**kwargs)
        print(_instance)
        return _instance[cls]
    return wrapper

@singleton
class A():
    def __init__(self,a):
        self.a=a
```

