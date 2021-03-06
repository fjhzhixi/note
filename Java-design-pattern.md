# Abstract Document

抽象文档设计模式

## 使用情景

1. 动态添加新属性
2. 实现低耦合
3. 灵活的组织树状结构的域

![abstract-document.png](https://github.com/iluwatar/java-design-patterns/blob/b6b4602baf5a12d8b76b3f8dd0284bae44230aa9/abstract-document/etc/abstract-document.png?raw=true)

## 实现要点

看不懂...





# Abstract-factory

抽象工厂设计模式

使用一个接口来创建一族有关联的产品

## 使用场景

1. 当存在一族产品系列时(产品之间有共处)
2. 产品本身独立于其产生过程
3. 运行时构造产品的需求

## 实现要点

1. 分析产品的组成要素
2. 构建各个要素的类
3. **构建工厂接口** , 其中主要是对各个要素的创建
4. 根据具体产品建立对应的工厂实例,根据具体实现要素建立方法

# Acyclic Visitor





# Observer

## 使用场景

一个目标对象的状态发生改变,所有的依赖对象都会得到通知

## 实现要点

* 一个目标对象`Subject`,其中有
  * 一个观察者列表,使用`attach(Observer o)`加入观察者
  * 一个`setStatus()`更新目标的状态,并且**在方法内调用`notifyAllObservers()`来通知所有观察者**
  * `notifyAllObservers()`方法中调用观察者列表中的每个观察者`observer.update()`来检查目标状态的改变
* 一个观察者接口,至少有`update()`
* 若干个观察者实例实现观察者接口

