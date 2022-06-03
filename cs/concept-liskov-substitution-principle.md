---
type: Concept
tags: OOP QoK-B
---

# Concept: 里氏替换原则 Liskov Substitution Principle

> 子类需要在和父类行为一致的基础上进行拓展。

里氏替换原则，The Liskov Substitution Principle，LSP，是对子类型的特别定义，指父类与子类可以互相替换而不会导致程序崩溃。是 [[concept-solid-principles|SOLID 原则]] 中的字母 L 所指代的原则，是对 [[concept-open-closed-principle|开闭原则]] 的扩展。

遵从开闭原则可以让我们写出健壮且可维护的代码，在此基础上遵从里氏替换原则，可以避免对实现的修改造成的其他副作用影响。

里氏替换原则由 Barbara Liskov 在她的 1987 年的关于“数据抽象”的演讲中提出，几年后在她同 Jeanatte Wing 一起发表的论文中定义如下：

Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T.

换而言之，这个原则定义了一个父类的实例对象可以被其子类的实例对象替代，并且不会中断程序的执行。这要求你的子类对象需要和父类的行为一致。

在子类中对方法进行重新需要接受和父类方法相同的签名，你可以复用父类的参数校验逻辑，但是不允许在子类中定义更严格的校验。否则在使用这个子类实例替换父类时，任何对此方法的调用都可能抛出异常。
