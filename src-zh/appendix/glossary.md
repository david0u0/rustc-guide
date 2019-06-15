<!--
# Appendix C: Glossary
-->

# 附录 C：术语表

<!--
The compiler uses a number of...idiosyncratic abbreviations and things. This
glossary attempts to list them and give you a few pointers for understanding
them better.
-->

编译器中使用了大量...怪异的缩写和词汇。本术语表旨在列出它们并提供一些解释和指引，
以便您更好地理解它们。

中文                    | 术语                    | 含义
------------------------|-------------------------|--------
AST                     | AST                     |  syntax crate 产生的抽象语法树（Abstract Syntax Tree），它非常严格地反映了用户使用的语法。
约束位                  | binder                  |  「约束位」是变量和类型声明的位置。例如，`<T>` 是泛型形参 `T` 在 `fn foo<T>(..)` 中的约束位，而 \|`a`\|` ...` 是形参 `a` 的约束位。详见[背景材料](./background.html#free-vs-bound)。
约束变量                | bound variable          |  「约束变量」是在表达式/项中声明的变量。例如，变量 `a` 就是闭包表达式 \|`a`\|` a * 2` 中的约束变量。详见[背景材料](./background.html#free-vs-bound)。
codegen                 | codegen                 |  将 MIR 翻译成 LLVM IR 的代码。
codegen 单元            | codegen unit            |  在产生 LLVM IR 时，我们会将 Rust 代码分成几组 codegen 单元。这些单元中的每一个都由 LLVM 独立处理以实现并行性。它们也是增量重用的单位。
完备性                  | completeness            |  完备性是类型论中的技术术语。完备性表示每个类型安全的程序都能通过类型检查。兼顾可靠性（Soundness）和完备性是非常困难的，通常可靠性更加重要（见「可靠性」）。
控制流图                | control-flow graph      |  程序控制流的一种表示方法。详见[背景材料](./background.html#cfg)。
CTFE                    | CTFE                    |  即编译期函数求值（Compile-Time Function Evaluation）。这是编译器在编译期求值 `const fn` 的能力。它是编译器常量求值系统的一部分（[详情](../const-eval.html)）。
cx                      | cx                      |  我们倾向于用「cx」作为上下文（context）的简写。另见 `tcx`、`infcx` 等。
DAG                     | DAG                     |  即有向无环图（directed acyclic graph），在编译时用于跟踪查询之间的依赖关系（[详情](../queries/incremental-compilation.html)）。
数据流分析              | data-flow analysis      |  静态分析可用来确定程序控制流中每个节点的属性是否正确。详见[背景材料](./background.html#dataflow)。
DefId                   | DefId                   |  一个标识定义的索引（见 `librustc/hir/def_id.rs`）。用于唯一地标识一个 `DefPath`。
双指针                  | Double pointer          |  带有附加元数据的指针。详见「胖指针」。
drop glue[^1]           | drop glue               |  （内部）编译器生成的指令，用于操纵对数据类型调用析构器（`Drop`）。
DST                     | DST                     |  动态大小的类型（Dynamically-Sized Type）。编译器无法静态地获知该类型在内存中的大小（例如 `str` 或 `[u8]`）。这种类型并未实现 `Sized` 且无法在栈上分配。它们只能作为结构体中的最后一个字段，且只能通过指针来使用（如 `&str` 或 `&[u8]`）。
先界定的生命周期        | early-bound lifetime    |  生存域在定义的位置被代换的生命周期。在元项的 `Generics` 中界定并通过 `Substs` 来代换。与**后界定生命周期**相对（[详情](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/enum.RegionKind.html#bound-regions)）。
空类型                  | empty type              |  见「不可居留类型」
胖指针                  | Fat pointer             |  一种两个字宽的指针，它保存了某个值的地址，以及使用该值所需的额外的信息。Rust 包含两种「胖指针」，分别是切片的引用和特质对象。切片的引用保存了切片的起始地址和长度。特质对象保存了一个值的地址和一个与该值对应的，指向该特质的实现的指针。「胖指针」也被称为「宽指针」或「双指针」。
自由变量                | free variable           |  「自由变量」即不在表达式或项中约束的变量。详见[背景材料](./background.html#free-vs-bound)。
'gcx                    | 'gcx                    |  即全域生命周期（[详情](../ty.html)）。
泛型（复数）            | generics                |  即一组在类型或元项上定义的泛型形参。
HIR                     | HIR                     |  上层 IR，将 AST 降级并脱糖（desugar）后产生（[详情](../hir.html)）。
HirId                   | HirId                   |  由一个 def-id 与一个「定义内偏移（intra-definition offset）」结合产生，用于在 HIR 中标识一个特定的节点。
HIR 映射                | HIR Map                 |  HIR 映射，通过 tcx.hir 访问，能够让你在 HIR 中快速导航，并在多种标识符之间互相转换。
ICE                     | ICE                     |  内部编译器错误（internal compiler error），在编译器崩溃时产生。
ICH                     | ICH                     |  增量编译散列值（incremental compilation hash）。ICH 作为 HIR 或 crate 元数据之类的指纹，用于检查它们是否被修改。它在增量编译时可用于检测 crate 的某部分是否被修改，确定是否应重新编译。
推导变量                | inference variable      |  「推导变量」是一种特殊的类型/生存域，在进行类型或生存域推导时，它用来表示尝试推导的东西。类比于代数中的 x。例如，如果我们尝试推导程序中某个变量的类型，那么就要创建一个推导变量来表示该未知的类型。
infcx                   | infcx                   |  推导上下文（见 `librustc/infer`）
存留                    | intern                  |  「存留」指先将确定且常用的常量数据（如字符串）存储下来，然后用标识符（例如一个 `Symbol`）来引用它，而非直接使用数据本身，以此节省内存。
IR                      | IR                      |  中间表示（Intermediate Representation）。编译器中一种通用的形式。在编译过程中，代码会从原始源码（ASCII 文本）转换为多种 IR。在 Rust 中，它们主要是 HIR、MIR 和 LLVM IR。每种 IR 都适用于某种计算集合。例如，MIR 适用于借用检查器，而 LLVM IR 则适用于 codegen，因为 LLVM 可以接受它。
IRLO                    | IRLO                    |  `IRLO` 或 `irlo` 有时用作 [internals.rust-lang.org](https://internals.rust-lang.org) 的缩写。
元项                    | item                    |  语言中的一种「定义」，例如 static、const、use 语句、模块、结构体等等。具体来说，它对应与 `Item` 类型。
语言元项                | lang item               |  表示语言本身固有概念的元项，其中包括：特殊的内建特质如 `Sync` 和 `Send`；表示操作的特质如 `Add`，由编译器调用的函数（[详情](https://doc.rust-lang.org/1.9.0/book/lang-items.html)）。
后界定的生命周期        | late-bound lifetime     |  生存域在调用的位置被代换的生命周期。在 HRTB 中界定并通过编译器中特定的函数来代换，例如 `liberate_late_bound_regions`。与**先界定生命周期**相对（[详情](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/enum.RegionKind.html#bound-regions)）。
局部 crate              | local crate             |  当前正在编译的 crate。
LTO                     | LTO                     |  链接期优化（Link-Time Optimizations）。一组由 LLVM 提供的优化，它只在最后链接二进制文件时执行。其中包括移除最终程序中不会被使用的函数等多种优化。_ThinLTO_ 是 LTO 的一种变体，旨在提高可扩展性和效率，但可能会牺牲一些优化。您也可以阅读 Rust 源码库中关于「FatLTO」的 Issue。「FatLTO」是为非 Thin LTO 的别称。LLVM 文档：[lto][lto] 和 [thinlto][thinlto]。
[LLVM]                  | [LLVM]                  |  （其实不算缩写 :-P）一个开源的编译器后端。它接受 LLVM IR 并输出原生二进制程序。很多编程语言（如 Rust）都可以实现一个输出 LLVM IR 的编译器前端，并用 LLVM 将该语言编译到所有 LLVM 支持的平台上。
记忆化                  | memoize                 |  记忆化是一种将计算结果（如纯函数调用的计算结果）存储起来以避免重复计算的方法。这是一种典型的执行速度和内存使用间的权衡。
MIR                     | MIR                     |  中层 IR，在通过 borrowck 和 codegen 进行类型检查后产生（[详情](../mir/index.html)）。
miri                    | miri                    |  一个 MIR 解释器，用于常量求值（[详情](../miri.html)）
正规化                  | normalize               |  常用术语，表示转换为更典范的形式，不过在 rustc 中一般指[关联类型的正规化](../traits/associated-types.html#normalize)。
新类型                  | newtype                 |  「新类型」是对其它类型的包装（例如，`struct Foo(T)` 就是 `T` 的「新类型」）。在 Rust 中它通常用于为索引提供一种更强的类型。
NLL                     | NLL                     | [非词法生命周期（non-lexical lifetimes）](../borrow_check/region_inference.html)，Rust 借用系统的一个扩展，使其基于控制流图。
node-id 或 NodeId       | node-id or NodeId       |  用于在 AST 或 HIR 中标识特定节点的索引，之后会被逐步淘汰并以 `HirId` 代之。
（证明）义务            | obligation              |  必须被特质系统证明的东西（[详情](../traits/resolution.html)）。
投影                    | projection              |  常用术语，表示「相对路径」，例如 `x.f` 是一种「字段投影」，而 `T::Item` 是一种「[关联类型投影](../traits/goals-and-clauses.html#trait-ref)」。
被提升常量              | promoted constants      |  从函数中提取并提升到静态作用域的常量。更多详情见[此节](../mir/index.html#promoted)。
提供者                  | provider                |  用于执行查询的函数（[详情](../query.html)）。
量化                    | quantified              |  在数学和逻辑学中，存在量化和全称量化用于提出像「是否存在类型 T 使其为真？」或「对于所有的类型 T 它是否都为真？」这样的问题。详见[背景材料](./background.html#quantified)。
查询                    | query                   |  在编译器可能存在的子计算过程（[详情](../query.html)）。
生存区                  | region                  |  生命周期的另一个术语，通常在书面用于或借用检查器中出现。
rib[^2]                 | rib                     |  名字求解器中的一种数据结构，用于跟踪多个名字所处的同一作用域（[详情](../name-resolution.html)）。
sess                    | sess                    |  编译器会话（session），用于存储整个编译期间使用的全局数据。
副表                    | side tables             |  由于 AST 和 HIR 一旦创建即不可变，因此我们通常以散列表的形式保存关于它们的额外的信息，并以特定节点的 id 来索引。
符文                    | sigil                   |  类似于关键字，但完全由非字母或数字的标记组成。例如，`&` 是一个表示引用的符文。
占位符                  | placeholder             |  **注意：skolemization 已被 placeholder 取代。** 一种处理「for-all」类型（例如 `for<'a> fn(&'a u32)`）的子定型（subtyping）以及解决高阶特质界定（HRTB，higher-ranked trait bounds，例如 `for<'a> T: Trait<'a>`）的方式。详见[占位符与全域](../borrow_check/region_inference.html#placeholders-and-universes)一节。
可靠性                  | soundness               |  可靠性是类型论中的技术术语。大致来说，如果一个类型系统是可靠的，且一个程序可通过类型检查，那么它就是类型安全的。例如，在 Safe Rust 中，我们无法将一个值强制赋予一个类型不匹配的变量（见「完备性」）。
（代码）区段            | span                    |  用户源码中的位置，主要用于错误报告。它是个形如「文件名/行/列」的元组，保存了起点/终点，也用于跟踪宏展开和编译脱糖（desugar）。所有这些都被打包成几个字节（实际上，它只是个表的索引）。更多信息见 Span 数据类型。
substs                  | substs                  |  对给定泛型类型或元项的代换（例如 `HashMap<i32, u32>` 中的 `i32` 和 `u32`）。
tcx                     | tcx                     |  即类型上下文（typing context），编译器的主要数据结构（[详情](../ty.html)）。
'tcx                    | 'tcx                    |  当前正在活动的推导上下文的生命周期（[详情](../ty.html)）。
特质引用                | trait reference         |  带有合适的输入类型/生命周期集合的特质的名字（[详情](../traits/goals-and-clauses.html#trait-ref)）。
（词法）标记            | token                   |  语法解析的最小单元。词法标记是分词后的产物（[详情](../the-parser.html)）。
[TLS]                   | [TLS]                   |  线程局部的存储（Thread-Local Storage）。定义的变量在每个线程中都有各自的一份副本（而非所有线程共享一份副本）。它会与 LLVM 有一些交互。不是所有平台都支持 TLS。
trans                   | trans                   |  将 MIR 翻译为 LLVM IR 的代码。重命名为 codegen。
特质引用                | trait reference         |  特质及其类型形参的值（[详情](../ty.html)）。
ty                      | ty                      |  类型（type）的内部表示（[详情](../ty.html)）。
UFCS                    | UFCS                    |  全域函数调用（Universal Function Call Syntax）。一种无歧义的调用方法的语法（[详情](../type-checking.html)）。
不可居留类型            | uninhabited type        |  一种 _无值_ 的类型。它与 ZST 不同，ZST 有刚好一个值。不可居留类型的一个例子是 `enum Foo {}`，它没有任何变体，因此无法被创建。编译器会将不可居留类型的的代码视作死代码，因此没有这样的值可被处理。`!`（即 never 类型）是一个不可居留类型。不可居留类型也叫做「空类型」。
upvar                   | upvar                   |  由闭包外部的闭包捕获的变量。
型变                    | variance                |  形变决定了泛型形参/生命周期形参的改变如何影响其子定型；例如，若 `T` 是 `U` 的子类型，则 `Vec<T>` 是 `Vec<U>` 的子类型，因为 `Vec` 对其泛型形参是 **协变（covariant）** 的。更一般的解释见[背景材料](./background.html#variance)。[型变](../variance.html)一章阐述了类型检查是如何处理型变的。
宽指针                  | Wide pointer            |  带有附加数据的指针。详见「胖指针」。
ZST                     | ZST                     |  零大小类型（Zero-Sized Type）。值的大小为 0 字节的类型。由于 `2^0 = 1`，因此这种类型只能有刚好一个值。例如，`()`（单元类型）就是一个 ZST。`struct Foo;` 也是一个 ZST。编程器可对 ZST 做出良好的优化。

[LLVM]: https://llvm.org/
[lto]: https://llvm.org/docs/LinkTimeOptimization.html
[thinlto]: https://clang.llvm.org/docs/ThinLTO.html
[TLS]: https://llvm.org/docs/LangRef.html#thread-local-storage-models

## 译注：

[^1]: 这里用了个双关。`Drop` 即 Rust 中的析构器 trait，用 `drop()` 方法来清理掉脱离作用域的值；
      glue 即胶水代码，用来粘合或填平不同的操作。drop glue 本意是滴胶，而 drop 又有删除，去掉的意思。
      到这里则引申出了，它是编译器生成的，用来做清理（drop）的胶水（glue）指令的意思。

[^2]: rib 本意是胸腔的肋骨，形状是从上往下逐渐增大，很像作用域由内向外层层扩大。
      然而不同于环状的是，它还有一个高度。作用域有个特点，就是即便一个作用域在另一个作用域之内，
      那么内部作用域里的变量也并不总是能访问外部作用的变量，有可能内部有个同名的变量把外部的屏蔽了。
      解决方案是，引入一个高度，让它变成个自上而下逐级变大的「栈」，并规定同级的内层可访问外层。
      如果内部能直接访问外部变量，那么它们在同一级上，作用域只分内外层；
      否则，就把内部的变量升高一级。在中文语境中，把它比作等高线或者汉诺塔似乎更加贴切。
