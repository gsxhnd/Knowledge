---
title: 装饰器详解
created: 2023-01-30 01:17:25
tags:
  - python
---

# 装饰器详解

## 闭包

在函数中再嵌套一个函数，并且引用外部函数的变量，这就是一个闭包了。

```python
def outer(x):
    def inner(y):
        return x + y
    return inner

print(outer(6)(5))
-----------------------------
>>>11
```

## 装饰器

官方文档 <https://docs.python.org/zh-cn/3.10/glossary.html#term-decorator>
返回值为另一个函数的函数，通常使用`@wrapper`语法形式来进行函数变换。 装饰器的常见例子包括 classmethod() 和 staticmethod()。

装饰器语法只是一种语法糖，以下两个函数定义在语义上完全等价:
```python
def f(arg):
    ...
f = staticmethod(f)

@staticmethod
def f(arg):
    ...
```

### 带参数装饰器

```python
def logging(level):
    def outwrapper(func):
        def wrapper(*args, **kwargs):
            print("[{0}]: enter {1}()".format(level, func.__name__))
            return func(*args, **kwargs)
        return wrapper
    return outwrapper

@logging(level="INFO")
def hello(a, b, c):
    print(a, b, c)

hello("hello,","good","morning")
-----------------------------
>>>[INFO]: enter hello()
>>>hello, good morning
```

### 类装饰器

装饰器也不一定只能用函数来写，也可以使用类装饰器，用法与函数装饰器并没有太大区别，实质是使用了类方法中的call魔法方法来实现类的直接调用。

```python
class logging(object):
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        print("[DEBUG]: enter {}()".format(self.func.__name__))
        return self.func(*args, **kwargs)

@logging
def hello(a, b, c):
    print(a, b, c)

hello("hello,","good","morning")
-----------------------------
>>>[DEBUG]: enter hello()
>>>hello, good morning
```

```python
class logging(object):
    def __init__(self, level):
        self.level = level

    def __call__(self, func):
        def wrapper(*args, **kwargs):
            print("[{0}]: enter {1}()".format(self.level, func.__name__))
            return func(*args, **kwargs)
        return wrapper

@logging(level="TEST")
def hello(a, b, c):
    print(a, b, c)

hello("hello,","good","morning")
-----------------------------
>>>[TEST]: enter hello()
>>>hello, good morning
```