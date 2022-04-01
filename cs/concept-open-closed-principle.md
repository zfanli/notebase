---
type: Concept
tags: OOP
---

# Concept: Open-closed Principle

> 定义接口提供额外一层抽象层，让调用代码和具体接口实现进行松耦合，并且接口通常不允许修改。

在面向对象编程中，开闭原则，The Open/Closed Principle，OCP，规定“软件中的实体（entity、module、function 等）应该开放扩展，但是封闭修改”。开闭原则是五个 [[concept-solid-principles|SOLID 原则]] 其中一个，SOLID 中的 O 代表开闭原则。

遵从这个原则，可以允许软件实体改变其行为，但是不需要修改到其源代码上。

Robert C. Martin 认为开闭原则是“面向对象设计中最重要的原则”。这个原则最早由 Bertrand Meyer 在他 1988 年出版的《面向对象软件构造》一书中提出。

开闭原则最初提议使用继承机制达成这一目的，但是经验告诉我们，继承会导致紧密的耦合，其子类会非常依赖父类的实现。

因此 Robert C. Martin 等人将开闭原则重新定义为**多态开闭原则**，Polymorphic Open/Closed Principle。使用接口而非父类，将允许你可以轻松替换不同的实现，并且对使用这些接口的地方不造成任何影响。其中接口本身是封闭修改的（即不允许被修改的），你可以提供新的接口实现来在你的软件中拓展功能性。

遵从开闭原则的主要优势在于使用接口来提供一层额外的抽象层，以此来达成松耦合的目的。接口的实现是独立的，被封装在接口下，不会和任何外部代码产生耦合。如果你认为同一个接口的不同实现存在代码可以复用，你可以选择使用继承机制或组合机制来完成复用的目的。