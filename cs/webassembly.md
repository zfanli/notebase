---
type: WebAssembly
---

# WebAssembly

WebAssembly，或者简称 WASM，可以理解为一种可以在现代浏览器环境下运行的新型代码，是一种类似底层汇编的语言，WASM 使用一种压缩的二进制格式，使其可以在浏览器中运行并获得接近原生环境的性能，WASM 提供 C/C++，C# 和 Rust 等语言的编译目标，让这些语言编写的应用可以直接在浏览器运行。WASM 可以和 JavaScript 一同在浏览器中工作。

> Assembly 实际就是汇编的意思，从这一层理解就比较容易了。

WASM 将给 Web 平台带来巨大的影响，WASM 提供一种方式在 Web 以接近原生的速度运行多种语言的代码，这在以前是不可能实现的事情。

WASM 被设计用来作为补足，与 JavaScript 一同运行，使用 WASM JavaScript API 你可以在 JavaScript App 中引用 WASM 模块来利用俩者的能力。这可以实现在同一个 App 中让你可以利用 WASM 的性能和能力，兼顾 JavaScript 的灵活性，并且不要求你会写 WASM 代码。

最后，最重要的是 WASM 现在已经是 W3C Web 标准的一部分。所有主流浏览器都已经实现了这个能力。
