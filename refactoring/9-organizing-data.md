## 第9章 重新组织数据

数据结构在程序中扮演着重要的角色。

将一个值同于多个不同的用途是催生混乱和 bug 的温床。

给变量起个好名字不容易但又非常重要。

### 拆分变量（Split Variable）

”循环变量“和”结果收集变量“是两个典型的例子。

如果变量被赋值超过一次，就意味着它们在函数中承担了一个以上的责任。此时可以考虑将变量分解为多个。

### 字段改名（Rename Field）

命名很重要，其中字段的命名格外重要。

数据结构是理解程序行为的关键。

### 以查询取代派生变量（Replace Derived Variable with Query）

可变数据是软件中最大的错误源头之一，对数据的修改常常导致丑陋的耦合。

尽量把可变数据的作用域限制在最小范围。

用查询取代变量，可以避免”元数据修改时忘记更新派生变量“的错误。

一种是对象风格，把一系列计算得出的属性包装在数据结构中；另一种是函数风格，将一个数据结构变换为另一个数据结构。

### 将引用对象改为值对象（Change Reference to Value）

值对象通常更容易理解，主要是因为它们是不可变的。

一般不可变的数据结构处理起来更容易。

如果希望在几个对象之间共享一个对象时，就需要使用引用对象。因为如果共享的数据需要更新，将其复制多份的做法就会遇到巨大的困难。