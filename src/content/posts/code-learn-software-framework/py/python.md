---
title: python基本语法特性
published: 2025-04-25
description: python一些语法特性和常用库的使用
tags:
  - 编程语言
  - python
category: 编程基础
draft: false
image: '/violet.png'
---

# 基础特性

## 继承和多态




## \_\_init\_\_和\_\_new\_\_区别联系

##  \_\_init\_\_ 方法是什么？


使用Python写过面向对象的代码的同学，可能对\_\_init\_\_方法已经非常熟悉了，\_\_init\_\_ 方法通常用在初始化一个类实例的时候。例如：

```python
class Person(object):
  def __init__(self, name, age):
    self.name = name self.age = age
  def __str__(self):
    return ‘<Person: %s(%s)>’ % (self.name, self.age)

if __name__ == ‘__main__’:
  Piglei = Person(‘Piglei’, 24) print(Piglei)
```

执行结果：

```python
<Person: Piglei(24)>
```

这样便是\_\_init\_\_最普通的用法了。但\_\_init\_\_其实不是实例化一个类的时候第一个被调用 的方法。当使用 Persion(name, age) 这样的表达式来实例化一个类时，最先被调用的方法 其实是 \_\_new\_\_ 方法。

### \_\_new\_\_方法是什么？

\_\_new\_\_方法接受的参数虽然也是和\_\_init\_\_一样，但\_\_init\_\_是在类实例创建之后调用，而 \_\_new\_\_方法正是创建这个类实例的方法。

```python
class Person(object):
  def __new__(cls, name, age):
    print(‘这是__new__’) return super(Person, cls).__new__(cls)
  def __init__(self, name, age):
    print(‘这是__init__’) self.name = name self.age = age
  def __str__(self):
    return ‘<Person: %s(%s)>’ % (self.name, self.age)

if __name__ == ‘__main__’:
  Piglei = Person(‘Piglei’, 24) print(Piglei)
```

执行结果：

```python
这是__new__ 这是__init__ <Person: Piglei(24)>
```

通过运行这段代码，我们可以看到，\_\_new\_\_方法的调用是发生在\_\_init\_\_之前的。其实当 你实例化一个类的时候，具体的执行逻辑是这样的：

1. p = Person(name, age)
2. 首先执行使用name和age参数来执行Person类的\_\_new\_\_方法，这个\_\_new\_\_方法会 返回Person类的一个实例（通常情况下是使用 super(Persion, cls).\_\_new\_\_(cls) 这样的方式），
3. 然后利用这个实例来调用类的\_\_init\_\_方法，上一步里面\_\_new\_\_产生的实例也就是 \_\_init\_\_里面的的 self
   
所以，\_\_init\_\_ 和 \_\_new\_\_ 最主要的区别在于： 

1. \_\_init\_\_ 通常用于初始化一个新实例，控制这个初始化的过程，比如添加一些属性， 做一些额外的操作，发生在类实例被创建完以后。它是实例级别的方法。
2. \_\_new\_\_ 通常用于控制生成一个新实例的过程。它是类级别的方法。 但是说了这么多，\_\_new\_\_最通常的用法是什么呢，我们什么时候需要\_\_new\_\_？

### \_\_new\_\_的作用

依照Python官方文档的说法，\_\_new\_\_方法主要是当你继承一些不可变的class时(比如int, str, tuple)， 提供给你一个自定义这些类的实例化过程的途径。 首先我们来看一下第一个功能，具体我们可以用int来作为一个例子： 假如我们需要一个永远都是正数的整数类型，通过集成int，我们可能会写出这样的代码：

```python
class PositiveInteger(int):
  def __init__(self, value):
    super(PositiveInteger, self).__init__()


i = PositiveInteger(-3) print(i)
```

但运行后会发现，结果根本不是我们想的那样，我们任然得到了-3。这是因为对于int这种 不可变的对象，我们只有重载它的\_\_new\_\_方法才能起到自定义的作用。 这是修改后的代码：

```python
class PositiveInteger(int):
  def __new__(cls, value):
    return super(PositiveInteger, cls).__new__(cls, abs(value))

i = PositiveInteger(-3) print(i)
```

通过重载\_\_new\_\_方法，我们实现了需要的功能。

### 用__new__来实现单例

事实上，当我们理解了\_\_new\_\_方法后，我们还可以利用它来做一些其他有趣的事情，比如实现 设计模式中的 单例模式(singleton) 。 因为类每一次实例化后产生的过程都是通过\_\_new\_\_来控制的，所以通过重载\_\_new\_\_方法，我们 可以很简单的实现单例模式。

```python
class Singleton(object):
  def __new__(cls):

# 关键在这里，每一次实例化时，我们都只会返回这同一个instance对象 if not hasattr(cls, ‘instance’):

cls.instance = super(Singleton, cls).__new__(cls)
return cls.instance

object1 = Singleton() 
object2 = Singleton()

object1.attr1 = ‘value1’ print(object1.attr1, object2.attr1) print(object1 is object2)
```

执行结果：

```python
value1 value1 True
```
可以看到obj1和obj2是同一个实例。
