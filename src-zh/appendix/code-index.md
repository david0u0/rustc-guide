<!--
# Appendix D: Code Index
-->

# 附录 D：代码索引

<!--
rustc has a lot of important data structures. This is an attempt to give some
guidance on where to learn more about some of the key data structures of the
compiler.
-->

rustc 中有大量重要的数据结构。本附录列出了一些编译器中关键的数据结构，给出了了解其细节的途径。

<!--
Item            |  Kind    | Short description           | Chapter            | Declaration
----------------|----------|-----------------------------|--------------------|-------------------
`BodyId` | struct | One of four types of HIR node identifiers | [Identifiers in the HIR] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/struct.BodyId.html)
`Compiler` | struct | Represents a compiler session and can be used to drive a compilation. | [The Rustc Driver and Interface] | [src/librustc_interface/interface.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_interface/interface/struct.Compiler.html)
`ast::Crate` | struct | A syntax-level representation of a parsed crate | [The parser] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/ast/struct.Crate.html)
`hir::Crate` | struct | A more abstract, compiler-friendly form of a crate's AST | [The Hir] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/struct.Crate.html)
`DefId` | struct | One of four types of HIR node identifiers | [Identifiers in the HIR] | [src/librustc/hir/def_id.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/def_id/struct.DefId.html)
`DiagnosticBuilder` | struct | A struct for building up compiler diagnostics, such as errors or lints | [Emitting Diagnostics] | [src/librustc_errors/diagnostic_builder.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_errors/struct.DiagnosticBuilder.html)
`DocContext` | struct | A state container used by rustdoc when crawling through a crate to gather its documentation | [Rustdoc] | [src/librustdoc/core.rs](https://github.com/rust-lang/rust/blob/master/src/librustdoc/core.rs)
`HirId` | struct | One of four types of HIR node identifiers | [Identifiers in the HIR] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/struct.HirId.html)
`NodeId` | struct | One of four types of HIR node identifiers. Being phased out | [Identifiers in the HIR] | [src/libsyntax/ast.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/ast/struct.NodeId.html)
`P` | struct | An owned immutable smart pointer. By contrast, `&T` is not owned, and `Box<T>` is not immutable. | None | [src/syntax/ptr.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/ptr/struct.P.html)
`ParamEnv` | struct | Information about generic parameters or `Self`, useful for working with associated or generic items | [Parameter Environment] | [src/librustc/ty/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/struct.ParamEnv.html)
`ParseSess` | struct | This struct contains information about a parsing session | [The parser] | [src/libsyntax/parse/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/parse/struct.ParseSess.html)
`Query` | struct | Represents the result of query to the `Compiler` interface and allows stealing, borrowing, and returning the results of compiler passes. | [The Rustc Driver and Interface] | [src/librustc_interface/queries.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_interface/queries/struct.Query.html)
`Rib` | struct | Represents a single scope of names | [Name resolution] | [src/librustc_resolve/lib.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_resolve/struct.Rib.html)
`Session` | struct | The data associated with a compilation session | [The parser], [The Rustc Driver and Interface] | [src/librustc/session/mod.html](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/session/struct.Session.html)
`SourceFile` | struct | Part of the `SourceMap`. Maps AST nodes to their source code for a single source file. Was previously called FileMap | [The parser] | [src/libsyntax_pos/lib.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/source_map/struct.SourceFile.html)
`SourceMap` | struct | Maps AST nodes to their source code. It is composed of `SourceFile`s. Was previously called CodeMap | [The parser] | [src/libsyntax/source_map.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/source_map/struct.SourceMap.html)
`Span` | struct  | A location in the user's source code, used for error reporting primarily | [Emitting Diagnostics] | [src/libsyntax_pos/span_encoding.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax_pos/struct.Span.html)
`StringReader` | struct | This is the lexer used during parsing. It consumes characters from the raw source code being compiled and produces a series of tokens for use by the rest of the parser | [The parser] |  [src/libsyntax/parse/lexer/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/parse/lexer/struct.StringReader.html)
`syntax::token_stream::TokenStream` | struct | An abstract sequence of tokens, organized into `TokenTree`s | [The parser], [Macro expansion] | [src/libsyntax/tokenstream.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/tokenstream/struct.TokenStream.html)
`TraitDef` | struct | This struct contains a trait's definition with type information | [The `ty` modules] |  [src/librustc/ty/trait_def.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/trait_def/struct.TraitDef.html)
`TraitRef` | struct | The combination of a trait and its input types (e.g. `P0: Trait<P1...Pn>`) | [Trait Solving: Goals and Clauses], [Trait Solving: Lowering impls]  |  [src/librustc/ty/sty.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/struct.TraitRef.html)
`Ty<'tcx>` | struct | This is the internal representation of a type used for type checking | [Type checking] | [src/librustc/ty/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/type.Ty.html)
`TyCtxt<'cx, 'tcx, 'tcx>` | struct | The "typing context". This is the central data structure in the compiler. It is the context that you use to perform all manner of queries | [The `ty` modules] | [src/librustc/ty/context.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/struct.TyCtxt.html)
-->

 条目 | 种类 | 简述 | 章节 | 声明
------|------|------|------|-----
`BodyId` | struct | 四种 HIR 节点标识符类型之一 | [HIR 中的标识符] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/struct.BodyId.html)
`Compiler` | struct | 表示编译器会话，可用于驱动编译。 | [rustc 驱动与接口] | [src/librustc_interface/interface.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_interface/interface/struct.Compiler.html)
`ast::Crate` | struct | 经解析的 crate 的语法层面的表示 | [解析器] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/ast/struct.Crate.html)
`hir::Crate` | struct | 更加抽象的，编译器友好的 crate 的 AST 形式 | [HIR] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/struct.Crate.html)
`DefId` | struct | 四种 HIR 节点标识符类型之一 | [HIR 中的标识符] | [src/librustc/hir/def_id.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/def_id/struct.DefId.html)
`DiagnosticBuilder` | struct | 用于构建编译器诊断，如错误或 lint 的结构体 | [触发诊断] | [src/librustc_errors/diagnostic_builder.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_errors/struct.DiagnosticBuilder.html)
`DocContext` | struct | rustdoc 在收集 crate 的文档时所用到的状态容器 | [Rustdoc] | [src/librustdoc/core.rs](https://github.com/rust-lang/rust/blob/master/src/librustdoc/core.rs)
`HirId` | struct | 四种 HIR 节点标识符类型之一 | [HIR 中的标识符] | [src/librustc/hir/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/hir/struct.HirId.html)
`NodeId` | struct | 四种 HIR 节点标识符类型之一，正在逐步淘汰 | [HIR 中的标识符] | [src/libsyntax/ast.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/ast/struct.NodeId.html)
`P` | struct | 被占有的不可变智能指针。对比来说，`&T` 未被占有，而 `Box<T>` 是不可变的 | 无 | [src/syntax/ptr.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/ptr/struct.P.html)
`ParamEnv` | struct | 泛型形参或 `Self` 的相关信息，在处理关联项或泛型项时使用 | [形参环境] | [src/librustc/ty/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/struct.ParamEnv.html)
`ParseSess` | struct | 此结构体包含有关解析器会话的信息 | [解析器] | [src/libsyntax/parse/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/parse/struct.ParseSess.html)
`Query` | struct | 表示 `Compiler` 接口的查询结果，允许偷用、借用和返回编译器每趟的结果 | [rustc 驱动与接口] | [src/librustc_interface/queries.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_interface/queries/struct.Query.html)
`Rib` | struct | 表示名字的单个作用域 | [名字求解] | [src/librustc_resolve/lib.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_resolve/struct.Rib.html)
`Session` | struct | 于编译回话关联的数据 | [解析器]，[rustc 驱动与接口] | [src/librustc/session/mod.html](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/session/struct.Session.html)
`SourceFile` | struct | `SourceMap` 的部分。将 AST 节点映射到它们的单个源文件的源码。曾名为 FileMap | [解析器] | [src/libsyntax_pos/lib.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/source_map/struct.SourceFile.html)
`SourceMap` | struct | 将 AST 节点映射到它们的源码。它由 `SourceFile` 组成。曾名为 CodeMap | [解析器] | [src/libsyntax/source_map.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/source_map/struct.SourceMap.html)
`Span` | struct  | 用户源码中的位置，主要用于错误报告 | [触发诊断] | [src/libsyntax_pos/span_encoding.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax_pos/struct.Span.html)
`StringReader` | struct | 解析过程中使用的词法分析器。它从待编译的原始源码中读入字符，产生一系列词法标记以供解析器使用 | [解析器] |  [src/libsyntax/parse/lexer/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/parse/lexer/struct.StringReader.html)
`syntax::token_stream::TokenStream` | struct | 抽象的词法标记序列，被组织为 `TokenTree` | [解析器]，[宏展开] | [src/libsyntax/tokenstream.rs](https://doc.rust-lang.org/nightly/nightly-rustc/syntax/tokenstream/struct.TokenStream.html)
`TraitDef` | struct | 此结构体包含特质的定义及其类型信息 | [`ty` 模块] |  [src/librustc/ty/trait_def.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/trait_def/struct.TraitDef.html)
`TraitRef` | struct | 特质及其输入类型的组合（例如 `P0: Trait<P1...Pn>`） | [特质求解：目标与子句]，[特质求解：底层实现]  |  [src/librustc/ty/sty.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/struct.TraitRef.html)
`Ty<'tcx>` | struct | 类型的内部表示，用于类型检查 | [类型检查] | [src/librustc/ty/mod.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/type.Ty.html)
`TyCtxt<'cx, 'tcx, 'tcx>` | struct | 「定型上下文（typing context）」。它是编译器中的核心数据结构。它是用于执行各种查询的上下文 | [`ty` 模块] | [src/librustc/ty/context.rs](https://doc.rust-lang.org/nightly/nightly-rustc/rustc/ty/struct.TyCtxt.html)

<!--
[The HIR]: ../hir.html
[Identifiers in the HIR]: ../hir.html#hir-id
[The parser]: ../the-parser.html
[The Rustc Driver and Interface]: ../rustc-driver.html
[Type checking]: ../type-checking.html
[The `ty` modules]: ../ty.html
[Rustdoc]: ../rustdoc.html
[Emitting Diagnostics]: ../diagnostics.html
[Macro expansion]: ../macro-expansion.html
[Name resolution]: ../name-resolution.html
[Parameter Environment]: ../param_env.html
[Trait Solving: Goals and Clauses]: ../traits/goals-and-clauses.html#domain-goals
[Trait Solving: Lowering impls]: ../traits/lowering-rules.html#lowering-impls
-->

[HIR]: ../hir.html
[HIR 中的标识符]: ../hir.html#hir-id
[解析器]: ../the-parser.html
[rustc 驱动与接口]: ../rustc-driver.html
[类型检查]: ../type-checking.html
[`ty` 模块]: ../ty.html
[Rustdoc]: ../rustdoc.html
[触发诊断]: ../diagnostics.html
[宏展开]: ../macro-expansion.html
[名字求解]: ../name-resolution.html
[形参环境]: ../param_env.html
[特质求解：目标与子句]: ../traits/goals-and-clauses.html#domain-goals
[特质求解：底层实现]: ../traits/lowering-rules.html#lowering-impls
