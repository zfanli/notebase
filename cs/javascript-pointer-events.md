---
type: JavaScript
tags: JavaScript Frontend
---

# 指针事件 Pointer Events

指针事件的出现是为了统一输入设备对页面的操作事件。

目前大部分网页假定用户使用鼠标作为指针的输入设备。然而自从很多设备开始支持数位笔、手写笔或触摸屏作为指针输入设备，那就有必要对目前的指针事件模型进行扩展。指针事件就是为了解决这一需求。

> Web worker 中不支持指针事件。

指针事件由指针输入设备触发，这些设备可以是鼠标、笔或者用户的手指操作等。

指针在这里指的是一种与硬件无关，且能指定一组特定屏幕坐标的设备。如果能统一指针设备的事件模型，就能简化 web 应用的开发，并且能够无关用户输入设备提供良好的用户体验。如果需要针对某种设别进行特殊处理，指针事件对象中有一个 `pointerType` 属性可以检查指针设备的类型，目前支持如下类型：

- `"mouse"`
  鼠标设备的事件。

- `"pen"`
  触控笔设备的事件。

- `"touch"`
  移动设备手指触摸事件。

指针事件需要能够处理通用的指针输入，通常使由鼠标触发的事件。结果来说，指针事件名称有意被设计为类似鼠标事件，指针事件模型的属性考虑迁移问题，实际上也是直接继承了鼠标事件，在此基础上添加了压力 pressure、接触轮廓 contact geometry、倾斜等属性。

## 术语

- `active buttons state` 当指针事件的 `buttons` 属性为非零的值时发生。比如输入设备为笔时，笔和转换器发生物理接触，或者至少浮在一个按钮上发生按压
- `active pointer` 一个可以产生事件的指针设备。指针是否激活取决于其是否能产生更多事件，比如一支笔在接触转换器（down 状态）时可以视作活跃的指针，因为其可以在抬起或滑动时会产生额外的事件
- `digitizer` 转换器，一种传感器可以侦测接触。通常来说转换器设备可以是触摸屏，能够感知笔或手指的输入操作。一些设备可以感知到输入设备的靠近，这种状态会被表达为鼠标的悬停
- `hit test` 浏览器确认指针事件的 `target` 对象的过程。一般来说，`target` 对象由指针在屏幕所处的位置和可视布局元素的层级来决定的
- `pointer` 一个与硬件无关的输入设备的表达，能够制定特定的一个或一组屏幕上的坐标。输入设备例如鼠标、笔和触摸接触
- `pointer capture` 指针捕获可以让事件重新绑定到特定的元素上，而不是浏览器进行 `hit test` 的结果
- `pointer event` 由指针设备触发的一组 DOM 事件

## 接口

指针事件的主要接口是 PointerEvent 接口，存在一个构造器和几种类型，以及相关的全局对象处理器。

标准也包括对 [[javascript-Element|Element]] 和 [[javascript-Navigator|Navigator]] 接口的扩展。

### PointerEvent

PointerEvent 继承了 MouseEvent 接口。

- `pointerId` 触发事件的指针的唯一标识
- `width` X 轴上所占宽度，单位是 CSS px，指针的接触轮廓宽度
- `height` Y 轴上所占长度，单位是 CSS px，指针的接触轮廓的高度
- `pressure` 0 到 1 的一个值，标准化的压力值
- `tangentialPressure` 标准化的正切压力，取值范围是 -1 到 1，0 表示控制的中立点
- `tiltX` 平面角度 -90 到 90 度，描述 Y-Z 平面和指针轴到 Y 轴平面之间到角度
- `tiltY` 平面角度 -90 到 90 度，描述 X-Z 平面到指针轴到 X 轴平面之间的角度
- `twist` 指针绕主轴顺时针旋转的角度，范围为 0 到 359
- `pointerType` 触发事件的设备类型
- `isPrimary` 当前指针是否是主要的指针

### 事件类型

指针事件类型有 10 种，其中 7 种和鼠标事件类型在语义上有重叠。

- `pointerover` 指针移动到一个元素的 `hit test` 边界之内
- `pointerenter` 当指针移动到一个元素或其子代的边界内触发。如果设备不支持悬停，这个事件会在 `pointerdown` 应该触发的时机触发
- `pointerdown` 指针进入 `active buttons state` 状态时触发
- `pointermove` 指针的坐标发生变化时触发。如果指针状态发生变化，并且没有其他事件能处理时，也会触发
- `pointerup` 指针退出 `active buttons state` 状态时触发
- `pointercancel` 浏览器发出这个事件表示这个指针无法再发生新的事件
- `pointerout` 多种情况触发：指针移出元素的 `hit test` 边界；如果设备不支持指针悬停，会在 `pointerup` 应该触发的时机触发；`pointercancle` 事件触发后触发；笔设备离开转换器可侦测的悬停范围时触发
- `pointerleave` 指针移出元素的 `hit test` 边界。对笔设备来说，这个事件在笔离开转换器可侦测的悬停范围时触发
- `getpointercapture` 元素接收到指针捕获时触发
- `lostpointercapture` 指针被一个指针捕获释放时触发

### Element 扩展

- `setPointerCapture()` 指定指针未来事件交由指定的元素处理
- `releasePointerCapture()` 释放之前设定的指针捕获

### Navigator 扩展

- `Navigator.maxTouchPoints` 属性表示同时支持的最多触点

### touch-action CSS 属性

`touch-action` CSS 属性会告诉浏览器释放应该应用默认触摸行为到指定的范围，这些行为例如缩放和平移。在触摸屏设备上使用部分指针事件时，浏览器的默认触摸行为会阻止由触摸行为产生的指针事件触发，需要禁用浏览器默认行为，指针事件才能正常工作。

```css
.target-selector {
  touch-action: none;
}
```
