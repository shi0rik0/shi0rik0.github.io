---
title: "用Python实现经典设计模式（更新中）"
date: 2024-12-06T10:19:20+08:00
draft: false
---

## 观察者模式（Observer Pattern）

所谓观察者模式，就是一个简单的消息通知机制。当一个对象想要发送消息时，订阅了这个消息的对象会收到通知。

假设我们有一个杂志社`MagazinePublisher`，它有很多订阅者`Subscriber`，当杂志社有新的杂志发布时，会通知所有的订阅者。

```python
class Subscriber:
    def __init__(self, name):
        self.name = name

    def update(self, message):
        print(f'{self.name} received message: {message}')

class MagazinePublisher:
    def __init__(self):
        self.subscribers = []

    def add_subscriber(self, subscriber):
        self.subscribers.append(subscriber)

    def remove_subscriber(self, subscriber):
        self.subscribers.remove(subscriber)

    def notify_subscribers(self, message):
        for subscriber in self.subscribers:
            subscriber.update(message)

subscriber1 = Subscriber('Alice')
subscriber2 = Subscriber('Bob')
publisher = MagazinePublisher()
publisher.add_subscriber(subscriber1)
publisher.add_subscriber(subscriber2)
publisher.notify_subscribers('New magazine is published!')
```

上面代码执行后的输出是：

```
Alice received message: New magazine is published!
Bob received message: New magazine is published!
```

## 简单工厂模式（Simple Factory Pattern）

所谓简单工厂模式，就是一个函数的返回类型会因为传入的参数不同而有所不同。

假设我们有一个汉堡店，可以根据顾客需求返回不同的汉堡。

```python
class Hamburger:
    def eat(self):
        print(f'Eating a {self._type} hamburger')

class BeefHamburger(Hamburger):
    def __init__(self):
        self._type = 'beef'

class ChickenHamburger(Hamburger):
    def __init__(self):
        self._type = 'chicken'

def make_hamburger(hamburger_type):
    if hamburger_type == 'beef':
        return BeefHamburger()
    elif hamburger_type == 'chicken':
        return ChickenHamburger()
    else:
        raise ValueError('Invalid hamburger type')

beef_hamburger = make_hamburger('beef')
beef_hamburger.eat()
chicken_hamburger = make_hamburger('chicken')
chicken_hamburger.eat()
```

上面代码执行后的输出是：

```
Eating a beef hamburger
Eating a chicken hamburger
```

## 单例模式（Singleton Pattern）

所谓单例模式，就是一个类只能有一个实例。在 Python 中，最简单的实现方式就是利用 module，因为一个 Python module 只会执行一次。

```python
# singleton.py
class Singleton:
    def __init__(self):
        if hasattr(self.__class__, '_created'):
            raise ValueError('Singleton instance already exists')
        self.__class__._created = True

    def do_something(self):
        print('Doing something')

singleton = Singleton()
```

```python
# main.py
from singleton import singleton

singleton.do_something()
```

## 策略模式（Strategy Pattern）

策略模式本质上就是利用了组合的思想，把类的一些功能“外包”给其他类。

（还在更新中……）
