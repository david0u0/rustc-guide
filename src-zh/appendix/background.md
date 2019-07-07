<!--
# Appendix B: Background topics
-->

# 附录 B：背景话题

<!--
This section covers a numbers of common compiler terms that arise in
this guide. We try to give the general definition while providing some
Rust-specific context.
-->

本节涵盖了本指南中出现的常见编译器术语。我们会在某些特定于 Rust
的上下文中给出这些术语的一般定义。

<a name="cfg"></a>

<!--
## What is a control-flow graph?
-->

## 什么是控制流图？

<!--
A control-flow graph is a common term from compilers. If you've ever
used a flow-chart, then the concept of a control-flow graph will be
pretty familiar to you. It's a representation of your program that
exposes the underlying control flow in a very clear way.
-->

控制流图（control-flow graph）是编译器中常见的术语。如果你曾使用过流程图，
那么控制流图的概念对你来说会很熟悉。它是程序的一种表示方式，
能够以非常清晰的方式展现底层控制流。

<!--
A control-flow graph is structured as a set of **basic blocks**
connected by edges. The key idea of a basic block is that it is a set
of statements that execute "together" – that is, whenever you branch
to a basic block, you start at the first statement and then execute
all the remainder. Only at the end of the block is there the
possibility of branching to more than one place (in MIR, we call that
final statement the **terminator**):
-->

控制流图由以边相连的一组**基本块（basic block）**构成。基本块的主要概念是
一组「一起」执行的语句，也就是说，只要你的分支跳到了基本块，
它就会从头到尾依次执行所有的语句。只有到基本块的最后才有可能分支到更多地方
（在 MIR 中，我们将最后一条语句称作**终止句（terminator）**）：

```mir
bb0: {
    statement0;
    statement1;
    statement2;
    ...
    terminator;
}
```

<!--
Many expressions that you are used to in Rust compile down to multiple
basic blocks. For example, consider an if statement:
-->

你在 Rust 中使用的很多表达式都会编译成多个基本块。例如，考虑以下 if 语句：

```rust,ignore
a = 1;
if some_variable {
    b = 1;
} else {
    c = 1;
}
d = 1;
```

<!--
This would compile into four basic blocks:
-->

它会被编译成四个基本块

```mir
BB0: {
    a = 1;
    if some_variable { goto BB1 } else { goto BB2 }
}

BB1: {
    b = 1;
    goto BB3;
}

BB2: {
    c = 1;
    goto BB3;
}

BB3: {
    d = 1;
    ...;
}
```

<!--
When using a control-flow graph, a loop simply appears as a cycle in
the graph, and the `break` keyword translates into a path out of that
cycle.
-->

在使用控制流图时，循环会简单地作为一个图中的环路出现，而 `break`
关键字则会被翻译成跳出此环路的一条路径。

<a name="dataflow"></a>

<!--
## What is a dataflow analysis?
-->

## 什么是数据流分析？

<!--
[*Static Program Analysis*](https://cs.au.dk/~amoeller/spa/) by Anders Møller
and Michael I. Schwartzbach is an incredible resource!
-->

[**静态程序分析（Static Program Analysis）**](https://cs.au.dk/~amoeller/spa/)，
作者 Anders Møller 和 Michael I. Schwartzbach，它是一个绝佳的资源。

*to be written*

<a name="quantified"></a>

<!--
## What is "universally quantified"? What about "existentially quantified"?
-->

## 什么是「全称量化」？「存在量化」呢？

*to be written*

<a name="variance"></a>

<!--
## What is co- and contra-variance?
-->

## 什么是协变和逆变？

<!--
Check out the subtyping chapter from the
[Rust Nomicon](https://doc.rust-lang.org/nomicon/subtyping.html).
-->

详见 [Rust 秘典](https://doc.rust-lang.org/nomicon/subtyping.html)
中的子定型（Subtyping）一章。

<!--
See the [variance](../variance.html) chapter of this guide for more info on how
the type checker handles variance.
-->

关于类型检查器如何处理型变的更多信息见本指南的
[型变（variance）](../variance.html)一章。

<a name="free-vs-bound"></a>

<!--
## What is a "free region" or a "free variable"? What about "bound region"?
-->

## 什么是「自由生存域」和「自由变量」？「约束生存域」呢？

<!--
Let's describe the concepts of free vs bound in terms of program
variables, since that's the thing we're most familiar with.

- Consider this expression, which creates a closure: `|a,
  b| a + b`. Here, the `a` and `b` in `a + b` refer to the arguments
  that the closure will be given when it is called. We say that the
  `a` and `b` there are **bound** to the closure, and that the closure
  signature `|a, b|` is a **binder** for the names `a` and `b`
  (because any references to `a` or `b` within refer to the variables
  that it introduces).
- Consider this expression: `a + b`. In this expression, `a` and `b`
  refer to local variables that are defined *outside* of the
  expression. We say that those variables **appear free** in the
  expression (i.e., they are **free**, not **bound** (tied up)).
-->

我们来描述一下程序变量的自由和约束的概念，因为它们是我们最熟悉的概念。

- 考虑此表达式，它创建了一个闭包：`|a, b| a + b`。在这里，`a + b` 中的 `a`
  和 `b` 指代该闭包被调用会时传入的参数。我们称 `a` 和 `b` 在该闭包中是
  **被约束（bound）**的，而闭包签名 `|a, b|` 是名字 `a` 和 `b` 的**约束位（binder）**
  （因为对 `a` 或 `b` 的任何引用都是指代它引入的变量）。

- 考虑此表达式 `a + b`。在该表达式中，`a` 和 `b` 均指代定义在该表达式**之外**
  的局部变量。我们称这些变量在该表达式中**自由出现（appear free）**
  （即它们是**自由（free）**的，而非**被约束（bound）**的（被束缚的））。

<!--
So there you have it: a variable "appears free" in some
expression/statement/whatever if it refers to something defined
outside of that expressions/statement/whatever. Equivalently, we can
then refer to the "free variables" of an expression – which is just
the set of variables that "appear free".
-->

所以现在你理解了：在某些「表达式、语句、还是别的什么」中的变量，
如果指代的是定义在该「表达式、语句、还是别的什么」之外的东西，那么它们就是
「自由出现」的。我们可以等价地称之为表达式中的「自由变量」，
毕竟它们就是一组「自由出现」的变量而已。

<!--
So what does this have to do with regions? Well, we can apply the
analogous concept to type and regions. For example, in the type `&'a
u32`, `'a` appears free.  But in the type `for<'a> fn(&'a u32)`, it
does not.
-->

那么，它们与生存域（region）有什么关系呢？我们可以将类似的概念应用到类型和生存域上来。
例如，在类型 `&'a u32` 中，`'a` 是自由出现的。但在类型 `for<'a> fn(&'a u32)` 中则不是。
