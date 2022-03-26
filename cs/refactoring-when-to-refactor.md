---
type: Refactoring
---

# Refactoring: When to Refactor

**三次原则**

1. 当你第一次开发一个东西，只管开发就成了。
2. 当你第二次做这件事，你可能因为要做重复的事情感觉为难，但是还是做了。
3. 当你第三次做这件事，考虑重构吧。

**加新功能时**

- 重构让你理解其他人的代码。如果要处理别人的脏代码，考虑先重构掉。干净的代码更容易把握。你会让自己的工作简单，还能让后面接手你工作的人方便。
- 重构会方便增加新功能。在干净的代码上操作起来更方便。

**改 Bug 的时候**

代码中的 Bug 都会在最脏最暗的地方。重构代码会让这些错误暴露出来。

Managers appreciate proactive refactoring as it eliminates the need for special refactoring tasks later. Happy bosses make happy programmers!

**在代码评审的时候**

代码评审是在代码上线前最后一次整理的机会。最好代码评审的时候带上写代码的人。对于简单的问题能快速解决掉，对于复杂的问题也能衡量一下解决所需的时间。
