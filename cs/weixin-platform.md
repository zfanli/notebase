---
type: Wx
tags: WebAssembly QoK-B
---

# 微信平台：小程序、公众号

微信作为一个流量入口，依托其存在的小程序和公众号平台是其官方推出的流量转化工具。想要让内容快速在这个平台上传播并转化为收益，就需要遵守微信制定的游戏规则来办。

> 说实话这种垄断平台制定的规则都存在一些问题，主要在于规则和平台生态过于耦合，如果你花了很多熟悉微信的游戏规则，那么一旦你退出这个圈子了，这些时间都等于白费了。没有一个标准化的规则可以适用所有平台，目前移动端孤岛内的游戏规则还是野蛮时代。
>
> 这也造成移动端兼容性问题频出。只能等有能与之分庭抗礼的竞争对手出现，生态竞争充分才可能出现标准化的希望。

这里记录一下要实现某些看似简单的效果所需的前提条件：

## 分享网页卡片预览

微信目前对于网页的分享不再提供卡片预览。

> 原因可能在于国内特殊的监管机制，任意网页都提供卡片预览，暴露更多信息的话，会造成滥用的风险。国内对这些风险的举措通常都是一刀切。

但是也并非所有页面都无法卡片预览，目前实现卡片预览的方式为：

- 将网页域名绑定为公众号的业务域名
- 对接微信的 js sdk，使用微信提供的接口定制卡片预览样式和内容
- 这个方法需要前后端对接，微信接口需要鉴权

方案成本在于这个能力不再是前端能独自完成，需要后端对微信的接口进行完整的对接封装。