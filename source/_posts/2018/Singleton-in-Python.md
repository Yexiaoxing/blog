---
title: Singleton in Python
date: 2018-04-16 02:13:11
tags:
  - Tutorials
  - Python
  - Design Pattern
  - Singleton
categories:
  - Tutorials
---

Recently, I went through some interviews from big name companies, such as Tencent and Bytedances. In the following posts, I would like to share some knowledge points, one for noting and another for your information.

# What is Singleton

Singleton pattern is a useful software design pattern that restricts the instantiation of a class to oe object. When only one exactly object is needed to use across the system, it becomes useful since it reduces the instantiation time.

A singleton class can and only can have one instance, and it should provide other objects a way to access the instance.

# Why Singleton

When you want to control the number of instances, and/or save system resources, you should use it.

Pros:

- No need for frequently creating and destorying instances
- Prevent from multiple occupation of resources, like file access

# How to make it

Simple explanation: Let the class itself responsible for controlling its instantiation.

The implementation of the singleton pattern shall,

- provide access to the instance;
- ensure that only one instance exists

Typically, this is done by,

- declaring all constructors to be private
- providing a static method that returns a reference to the instance.

We should be aware that, if multi-threads is used in project, a double check (synchronized lock) should be used. Otherwise, if multiply threads request the instance, more than one instance may be instantized.

# Sample codes

In Wikipedia, many examples are provided, but I still want to share some other examples in Python.

## Use modules

By nature, all modules are singletons because Python will cache the module initialization (the byte-compiled .pyc files)

    A program doesn't run any faster when it is read from a ‘.pyc’ or ‘.pyo’ file than when it is read from a ‘.py’ file; the only thing that's faster about ‘.pyc’ or ‘.pyo’files is the speed with which they are loaded.

    When a script is run by giving its name on the command line, the bytecode for the script is never written to a ‘.pyc’ or ‘.pyo’ file. Thus, the startup time of a script may be reduced by moving most of its code to a module and having a small bootstrap script that imports that module. It is also possible to name a ‘.pyc’ or ‘.pyo’file directly on the command line.

So if we want to quickly make a singleton, modules are your good friend. For example...

```python
# mysingleton.py
class MySingleton(object):
    def foo(self):
        pass

my_singleton = My_Singleton()
```

Save it as mysingleton.py, and import it using...

```python
from mysingleton import my_singleton


my_singleton.foo()
```

Perfect!

## Use Decorator

A decorator is the name used for a software design pattern. Decorators dynamically alter the functionality of a function, method, or class without having to directly use subclasses or change the source code of the function being decorated.

Here we will use a decorator to decorate the singleton class.

```python
from functools import wraps

__instances = {}
def singleton(cls):
    @wraps(cls)
    def getInstance(*args, **kwargs):
        instance = __instances.get(cls, None)
        if not instance:
            instance = cls(*args, **kwargs)
            __instances[cls] = instance
        return instance
    return getInstance


@singleton
class MySingleton:
    def foo(self):
        pass

MySingleton().foo()
```

## Use `__new__`

`__new__` is a special method to control the instantiation of instances. We can utilize it to make our class singleton.

```python
class Singleton:
    __instance = None
    def __new__(cls, *args, **kwargs):
        if not cls.__instance:
            cls.__instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls.__instance

class MySingleton(Singleton):
    def foo(self):
        pass

MySingleton().foo()
```

Here we make a superclass for inheritance.

## Use metaclass

To be honest, metaclass is the most difficult-to-understand concept in Python... If you want to learn it, check [https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python).

A metaclass is the class of a class. Think about instances of class, we need to class to create an instance. What if we want to create a class? Bingo, we need a metaclass to create a class. Using a metaclass, we can achieve...

- intercept the creation of class
- modify the definition of a class
- return the modified class

I won't go too far here, and only some code will be here. You will find it similar to the previous one :)

```python
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]

class MySingleton(metaclass=Singleton):
    def foo(self):
        pass

MySingleton().foo()
```

## Summary

Use modules, which should be enough for most cases.

# Other languages

In Java, since I know nothing about it, I recommend you to refer to [this post](http://wuchong.me/blog/2014/08/28/how-to-correctly-write-singleton-pattern/) (in Chinese).

For other languages, use Google :)

# References

- Wikipedia contributors. (2018, March 29). Singleton pattern. In Wikipedia, The Free Encyclopedia. Retrieved 18:29, April 15, 2018, from [https://en.wikipedia.org/w/index.php?title=Singleton_pattern&oldid=833049720](https://en.wikipedia.org/w/index.php?title=Singleton_pattern&oldid=833049720)
- [http://funhacks.net/2017/01/17/singleton](http://funhacks.net/2017/01/17/singleton)