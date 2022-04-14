---
type: Concept
tags: OOP
---

# Concept: 接口隔离原则 Interface Segregation Principle

接口隔离原则，The Interface Segregation Principle，ISP，指将接口划分为更小单位且更加特定的接口，用户可以只需要关注其需要的部分接口。[[concept-solid-principles|SOLID 原则]] 中的字母 I 代表接口隔离原则。

接口隔离原则由 Robert C. Martin 在为 Xerox 提供咨询时，帮助他们为新打印机构建软件系统时提出，其定义为：

“Clients should not be forced to depend upon interfaces that they do not use.”

这个原则听上去很简单，但是随着业务系统的发展，越来越多的功能加入进来时，有时会很轻易违背这个原则。同 [[concept-single-responsibility-principle|单一职责原则]] 一样，接口隔离原则的目标在于通过将软件切分为多个独立的部分，从而降低业务需求变更时引起的副作用和修改频度。

比如一个系统使用了很多年，但是用户定期会提出新的需求。从商业角度来说这是一件好事，但是从技术角度来说，每一次修改都会承受一定风险。新的功能通常会尝试加到现有的接口中，即使职责不同，即使新开一个接口或许是更好的方式。这就会造成接口污染，然后造成接口膨胀，一个接口实现多个职责。
