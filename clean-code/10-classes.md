## 第10章 类

- [Clean Code 目录](./index.md)

类应该短小——其权责应足够小。

类的名称应描述其权责。

    命名正式帮助判断类的长度的第一个手段。
    如果无法为某个类命以精确的名称，这个类大概就太长了。
    如果类名中包括含义模糊的词，如Processor或Manager或Super，这种现象往往说明不恰当的权责聚集。

单一权责原则（Single Responsibility Principle）认为，类或模块有且只有一条加以修改的理由。

    类只应有一个权责——只有一条修改的理由。
    鉴别权责（修改的理由）帮助我们在代码中认识到并创建出更好的抽象。

让软件能工作和让软件保持整洁，是两种截然不同的工作。

    一定规模的系统都会包括大量逻辑和复杂性。
    管理这种复杂性的首要目标就是加以组织，以便开发者知道到哪儿能找到东西，并且在某个特定时间只需要理解直接相关的复杂性。
    反之，拥有巨大的、多目的类的系统，总是让我们在目前不需要了解的一大堆东西中艰难跋涉。

系统应该有许多短小的类而不是少量巨大的类组成。

    每个小类封装一个权责，只有一个修改的原因，并与少数其他类一起协同达成期望的系统行为。

类应该只有少量实体变量。

内聚性高，意味着类中的方法和变量互相依赖、互相结合成一个逻辑整体。

当类丧失了内聚性，就拆分它！

    将大函数拆分为许多小函数，往往也是将类拆分为多个小类的时机。
    
在整洁的系统中，我们对类加以组织，以降低修改的风险。

    对类的任何修改都有可能破坏类中的其他代码。必须全面重新测试。

一旦打开了类，就应当修正设计方案。

开放-封闭原则（Open Close Priciple）：类应当对扩展开放，对修改封闭。

在理想系统中，我们通过扩展系统而非修改现有代码来添加新特性。

    具体类包含实现细节，而抽象类则只呈现概念。
    我们可以借助接口和抽象类来隔离这些细节带来的影响。

通过降低连接度，我们的类就遵循了另一条类设计原则，依赖倒置原则（Dependency Inversion Principle）。

    DIP认为类应当依赖于抽象而不是依赖于具体细节。