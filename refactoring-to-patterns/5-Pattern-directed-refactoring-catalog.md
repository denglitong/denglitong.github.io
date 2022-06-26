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