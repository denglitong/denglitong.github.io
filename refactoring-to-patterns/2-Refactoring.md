## 第2章 重构

- [Refactoring to Patterns 目录](./index.md)

### 何谓重构

重构是一种“保持行为的转换”，是对软件内部结构的改善，目的是在不改变软件的可见行为的情况下，使其更易理解，修改成本更低。

重构过程包括去除重复、简化复杂逻辑和澄清模糊的代码。

要保证重构的安全性，必须要有全面的测试。

循序渐进地进行重构有助于防止增加缺陷。重构最好是持续而不是分阶段地运行。重构实践必须和谐地适应业务上的轻重缓急。

### 重构的动机

1. 使新代码的增加更容易。

    技术债务迟早还是要通过重构来偿还的。

2. 改善既有代码的设计。

3. 对代码理解更透彻

    如果代码本身不清晰，就说明存在坏味道，需要通过重构清楚，而不是用注解来掩饰。

4. 提高编程的趣味性

    经常重构，使编写出的代码不那么讨厌！

### 众目睽睽

《独立宣言》中有一句：“我们认为这些真理是不言而喻的”。

所有优秀的著述，都是不断修改而成的。

修改意味着重新审视。

某个人的工作如果能经过多个人进行修改，将得到显著的改进。

### 可读性好的代码

可读性好的代码读起来就像自然语言，可以将重要代码与分散注意力的代码分离开来。

任何傻瓜都会编写计算机能理解的代码。好的程序员能够编写人能够理解的代码。

### 保持清晰

保持代码清晰类似于保持房间整洁。

要保持代码清晰，必须持续地去除重复，简化和澄清代码。

清晰的代码能产生更好的设计，而更好的设计将使开发过程更加迅速。

采取更小的、更安全的步骤比采取更大的步骤更能快速达到目标。

### 设计欠账

使用重构的技术语言和大多数管理人员交流，效果都不好。相反，使用设计欠账隐喻则有效的多。

设计欠账是指无法一致做如下 3 件事情：去除重复、简化代码、澄清代码的意图。

人类无法一次写就完美的代码，问题只在于何时该偿还设计欠账。

用金融术语来说，如果不偿还债务，就会产生滞纳金。如果不偿还滞纳金，滞纳金就会越积越多。随着时间推移，还清这些欠账就变成不可实现的梦。

### 演变出新的架构

持续重构对整个过程至关重要，因为它能够使框架和应用程序分离开。

### 复合重构与测试驱动的重构

复合重构（composite refactoring）就是由多个低层次重构组成的高层次重构。低层次重构所完成的许多工作都涉及代码的搬移。

测试是重构不可分割的一部分，不允许测试很难自信地应用低层次重构。

当低层次重构无法改善设计时，采用测试驱动的重构能够帮助我们安全且有效地得到更佳的设计。

替换算法重构是最适合使用测试驱动重构方式来实现重构的绝佳例子。

用 Builder 封装 Composite 重构是测试驱动重构的另一个例子。

通过测试驱动的重构完成的“重新实现和替换”技术，是重构的一种有效方式。

### 复合重构的优点

1. 描述了重构顺序的完整计划

    复合重构的顺序在改善涉及方面比自认为合适的重构顺序更加安全、有效且更高效。

2. 能够提示不明显的设计方向

    复合重构引导你从源头走到目的地。

3. 促进对实现模式的深入思考

    实现模式没有一定之规，思考模式的各种实现方案是大有裨益的，对于理解不同设计问题的模式尤其必要。

### 重构工具

IDE 中为主流语言开发了自动重构工具，通过执行一系列自动的低层次重构，让实现设计转换变得更加容易。

但是，模式代码的生成工具所提供的的是饮鸩止渴之道，很容易使代码过度设计。而且它们所生成的代码不包含任何测试。相比之下，通过能够发现小的改善涉及的措施，而且能够安全地实现、趋向或者去除模式。

一般而言，如果代码的测试覆盖不够，重构其实不会获得太好的结果。
