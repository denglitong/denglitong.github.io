## 第5章 模式导向的重构目录

- [Refactoring to Patterns 目录](./index.md)

### XML Builder

- 用 Composite 替换隐含树
- 用 Factory Method 引入多态创建
- 用 Builder 封装 Composite
- 将聚集操作搬移到 Collecting Parameter
- 通过 Adapter 统一接口


In the Collecting Parameter idiom a collection (list, map, etc.) is passed repeatedly as a parameter to a method which adds items to the collection.

### HTML Parser

- 将装饰功能搬移到 Decorator
- 将创建知识搬移到 Factory
- 提取 Composite
- 将聚集操作搬移到 Visitor

[Visitor is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.](https://refactoring.guru/design-patterns/visitor)


### 贷款风险计算程序

- 链构造方法
- 用 Creation Method 替换构造函数
- 用 Strategy 替换条件逻辑

重构只是一个起点，在重构过程中，你一定可以找出代码中的个中缺陷。

### 学习顺序

1. 用 Creation Method 替换构造函数；链构造函数

    Creation Method is simply a static or nonstatic method that creates and returns an object instance.

2. 用 Factory 封装类

    In Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.

3. 用 【Factory Method](https://refactoring.guru/design-patterns/factory-method) 引入多态创建

    Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

    The Factory Method pattern suggests that you replace direct object construction calls (using the new operator) with calls to a special factory method. Don’t worry: the objects are still created via the new operator, but it’s being called from within the factory method.

4. 用 [Strategy](https://refactoring.guru/design-patterns/strategy) 替换条件逻辑

    Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

    The Strategy pattern suggests that you take a class that does something specific in a lot of different ways and extract all of these algorithms into separate classes called strategies.

    The original class, called context, must have a field for storing a reference to one of the strategies. The context delegates the work to a linked strategy object instead of executing it on its own.

5. 形成 Template Method

    In Template pattern, an abstract class exposes defined way(s)/template(s) to execute its methods. Its subclasses can override the method implementation as per need but the invocation is to be in the same way as defined by an abstract class. This pattern comes under behavior pattern category.

6. 组合方法
7. 用 [Composite](https://refactoring.guru/design-patterns/composite) 替换隐含树

    Composite is a structural design pattern that lets you compose objects into tree structures and then work with these structures as if they were individual objects.

    Using the Composite pattern makes sense only when the core model of your app can be represented as a tree.

8. 用 Builder 封装 Composite

    A Builder [DP] performs burdensome or complicated construction steps on behalf of a client. A common motivation for refactoring to a Builder is to simplify client code that creates complex objects.

    Builders often encapsulate Composites [DP] because the construction of Composites can frequently be repetitive, complicated, or error-prone.

9. 将聚集操作搬移到 Collecting Parameter

    In the CollectingParameter idiom a collection (list, map, etc.) is passed repeatedly as a parameter to a method which adds items to the collection.

10. 提取 Composite
11. 用 Composite 替换一/多之分
12. 提取 Adapter；通过 Adapter 统一接口
13. 用类替换类型代码
14. 用 State 替换状态改变条件语句
15. 引入 NullObject
16. 内联 Singleton；用 Singleton 限制实例化
17. 用 Observer 替换硬编码的通知
18. 将装饰功能搬移到 Decorator；统一接口；提取参数
19. 将创建知识搬移到 Factory
20. 将聚集操作搬移到 Visitor
21. 用 Interpreter 替换隐式语言