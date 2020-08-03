# 调试编译器
[debugging]: #debugging

本章节涵盖一些调试编译器的小技巧，不论你在处理什麽问题，这些技巧应该都能派上用场。一些其它章节则可能涵盖编译器的特定面向（例如，[Queries Debugging and Testing chapter](./incrcomp-debugging.html) 或 [LLVM Debuggin chapter](./codegen/debugging.md)。

<!--This chapter contains a few tips to debug the compiler. These tips aim to be
useful no matter what you are working on.  Some of the other chapters have
advice about specific parts of the compiler (e.g. the [Queries Debugging and
Testing
chapter](./incrcomp-debugging.html) or
the [LLVM Debugging chapter](./codegen/debugging.md)).-->

## `-Z` 旗标

编译器有许多 `-Z` 旗标，它们并不稳定，只能在 nightly 版使用。许多 `-Z` 旗标可以帮助你调试编译器。如果你想列出完整的 `-Z` 旗标，请使用 `-Z help`。

`-Z verbose` 就是个好用的旗标，它会令编译器印出更多讯息，可能有助于调试。

<!-- The compiler has a bunch of `-Z` flags. These are unstable flags that are only
enabled on nightly. Many of them are useful for debugging. To get a full listing
of `-Z` flags, use `-Z help`.

One useful flag is `-Z verbose`, which generally enables printing more info that
could be useful for debugging. -->

## 取得回溯记录 (backtrace)
[getting-a-backtrace]: #getting-a-backtrace

当你遭遇到 ICE (panic in the compiler)，你可以设置 `RUST_BACKTRACE=1` 以取得该次 `panic!` 的栈追踪，如同普通的 rust 程式。如果我没记错，回溯记录 **不适用** 于 Mac 及 MigGW，很抱歉。如果你遭遇困难，或是回溯记录充满 `unknown`，你可能会想弄个 Linux 或 MSVC on Windows 来用。

<!-- When you have an ICE (panic in the compiler), you can set
`RUST_BACKTRACE=1` to get the stack trace of the `panic!` like in
normal Rust programs.  IIRC backtraces **don't work** on Mac and on MinGW,
sorry. If you have trouble or the backtraces are full of `unknown`,
you might want to find some way to use Linux or MSVC on Windows.

In the default configuration, you don't have line numbers enabled, so the
backtrace looks like this: -->

在默认配置下，你无法看见代码行数，因此回溯记录应如下：

```text
stack backtrace:
   0: std::sys::imp::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::_print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::begin_panic
   (~~~~ LINES REMOVED BY ME FOR BREVITY ~~~~)
  32: rustc_typeck::check_crate
  33: <std::thread::local::LocalKey<T>>::with
  34: <std::thread::local::LocalKey<T>>::with
  35: rustc::ty::context::TyCtxt::create_and_enter
  36: rustc_driver::driver::compile_input
  37: rustc_driver::run_compiler
```

如果想得到栈追踪的行数，可以在 config.toml 中设定 `debug = true`，并重新建置编译器（`debuginfo-level = 1` 也会打上行数，但 `debug = true` 会给出完整的调试讯息）。如此一来回溯讯息应如下：

<!-- If you want line numbers for the stack trace, you can enable `debug = true` in
your config.toml and rebuild the compiler (`debuginfo-level = 1` will also add
line numbers, but `debug = true` gives full debuginfo). Then the backtrace will 
look like this: -->

```text
stack backtrace:
   (~~~~ LINES REMOVED BY ME FOR BREVITY ~~~~)
             at /home/user/rust/src/librustc_typeck/check/cast.rs:110
   7: rustc_typeck::check::cast::CastCheck::check
             at /home/user/rust/src/librustc_typeck/check/cast.rs:572
             at /home/user/rust/src/librustc_typeck/check/cast.rs:460
             at /home/user/rust/src/librustc_typeck/check/cast.rs:370
   (~~~~ LINES REMOVED BY ME FOR BREVITY ~~~~)
  33: rustc_driver::driver::compile_input
             at /home/user/rust/src/librustc_driver/driver.rs:1010
             at /home/user/rust/src/librustc_driver/driver.rs:212
  34: rustc_driver::run_compiler
             at /home/user/rust/src/librustc_driver/lib.rs:253
```

## 取得错误的回溯记录
[getting-a-backtrace-for-errors]: #getting-a-backtrace-for-errors

如果想回溯至编译器产生错误的确切段落，可以设定 `-Z treat-err-as-bug=n`，这会让编译器跳过 `n` 个错误或 `delay_span_bug`（一个类似报错的函式），再下一次就会 panic。如果不带 `=n`，编译器会将 `n` 设为 `0`，因此遇到一个错误就会 panic。

<!-- If you want to get a backtrace to the point where the compiler emits
an error message, you can pass the `-Z treat-err-as-bug=n`, which
will make the compiler skip `n` errors or `delay_span_bug` calls and then
panic on the next one. If you leave off `=n`, the compiler will assume `0` for
`n` and thus panic on the first error it encounters.

This can also help when debugging `delay_span_bug` calls - it will make
the first `delay_span_bug` call panic, which will give you a useful backtrace.

For example: -->

这也有助于 `delay_span_bug` 被呼叫的情境──它会令程式在 `delay_span_bug` 被呼叫第一次时 panic，并且印出有用的回溯记录。

例如：

```bash
$ cat error.rs
fn main() {
    1 + ();
}
```

```bash
$ ./build/x86_64-unknown-linux-gnu/stage1/bin/rustc error.rs
error[E0277]: the trait bound `{integer}: std::ops::Add<()>` is not satisfied
 --> error.rs:2:7
  |
2 |     1 + ();
  |       ^ no implementation for `{integer} + ()`
  |
  = help: the trait `std::ops::Add<()>` is not implemented for `{integer}`

error: aborting due to previous error

$ # Now, where does the error above come from?
$ RUST_BACKTRACE=1 \
    ./build/x86_64-unknown-linux-gnu/stage1/bin/rustc \
    error.rs \
    -Z treat-err-as-bug
error[E0277]: the trait bound `{integer}: std::ops::Add<()>` is not satisfied
 --> error.rs:2:7
  |
2 |     1 + ();
  |       ^ no implementation for `{integer} + ()`
  |
  = help: the trait `std::ops::Add<()>` is not implemented for `{integer}`

error: internal compiler error: unexpected panic

note: the compiler unexpectedly panicked. this is a bug.

note: we would appreciate a bug report: https://github.com/rust-lang/rust/blob/master/CONTRIBUTING.md#bug-reports

note: rustc 1.24.0-dev running on x86_64-unknown-linux-gnu

note: run with `RUST_BACKTRACE=1` for a backtrace

thread 'rustc' panicked at 'encountered error with `-Z treat_err_as_bug',
/home/user/rust/src/librustc_errors/lib.rs:411:12
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose
backtrace.
stack backtrace:
  (~~~ IRRELEVANT PART OF BACKTRACE REMOVED BY ME ~~~)
   7: rustc::traits::error_reporting::<impl rustc::infer::InferCtxt<'a, 'gcx,
             'tcx>>::report_selection_error
             at /home/user/rust/src/librustc/traits/error_reporting.rs:823
   8: rustc::traits::error_reporting::<impl rustc::infer::InferCtxt<'a, 'gcx,
             'tcx>>::report_fulfillment_errors
             at /home/user/rust/src/librustc/traits/error_reporting.rs:160
             at /home/user/rust/src/librustc/traits/error_reporting.rs:112
   9: rustc_typeck::check::FnCtxt::select_obligations_where_possible
             at /home/user/rust/src/librustc_typeck/check/mod.rs:2192
  (~~~ IRRELEVANT PART OF BACKTRACE REMOVED BY ME ~~~)
  36: rustc_driver::run_compiler
             at /home/user/rust/src/librustc_driver/lib.rs:253
$ # Cool, now I have a backtrace for the error
```

## 取得日誌输出
[getting-logging-output]: #getting-logging-output

编译器使用了以下的 crate 处理日誌：

* [log]
* [env-logger]: 参照连结以瞭解完整的 `RUST_LOG` 语法

[log]: https://docs.rs/log/0.4.6/log/index.html
[env-logger]: https://docs.rs/env_logger/0.4.3/env_logger/

编译器时常会呼叫 `debug!` 在各处印出日誌讯息，它们十分有用，至少能帮你限缩臭虫的位置（甚至直接找到它），或釐清为何编译器做了某件事。

为了印出日誌，你得设定环境变数 `RUSTC_LOG` 来筛选讯息。例如，执行 `RUSTC_LOG=module::path rustc my-file.rs` 可取得特定模块的日誌。所有 `debug!` 都将输出至标准错误流(standard error)。

**需注意，除非你使用了非常严格的筛选条件，否则将会产生大量的输出，因此你应该用你能想到的最特定的模块（或多个模块，这时你该用逗号来分隔它们）**。通常你会想把标准错误流导向一个档案，并以文本编辑器检视之。

<!-- The compiler has a lot of `debug!` calls, which print out logging information
at many points. These are very useful to at least narrow down the location of
a bug if not to find it entirely, or just to orient yourself as to why the
compiler is doing a particular thing.

To see the logs, you need to set the `RUSTC_LOG` environment variable to
your log filter, e.g. to get the logs for a specific module, you can run the
compiler as `RUSTC_LOG=module::path rustc my-file.rs`. All `debug!` output will
then appear in standard error.

**Note that unless you use a very strict filter, the logger will emit a lot of
output, so use the most specific module(s) you can (comma-separated if
multiple)**. It's typically a good idea to pipe standard error to a file and
look at the log output with a text editor.

So to put it together. -->

小结一下：

```bash
# This puts the output of all debug calls in `librustc/traits` into
# standard error, which might fill your console backscroll.
$ RUSTC_LOG=rustc::traits rustc +local my-file.rs

# This puts the output of all debug calls in `librustc/traits` in
# `traits-log`, so you can then see it with a text editor.
$ RUSTC_LOG=rustc::traits rustc +local my-file.rs 2>traits-log

# Not recommended. This will show the output of all `debug!` calls
# in the Rust compiler, and there are a *lot* of them, so it will be
# hard to find anything.
$ RUSTC_LOG=debug rustc +local my-file.rs 2>all-log

# This will show the output of all `info!` calls in `rustc_trans`.
#
# There's an `info!` statement in `trans_instance` that outputs
# every function that is translated. This is useful to find out
# which function triggers an LLVM assertion, and this is an `info!`
# log rather than a `debug!` log so it will work on the official
# compilers.
$ RUSTC_LOG=rustc_trans=info rustc +local my-file.rs
```

### 如何从二进制档中保留/移除 `debug!` 和 `trace!`
虽然每次建置编译器都会包含 `error!`, `warn!` 和 `info!`，但 `debug!` 和 `trace!` 只会在 config.toml 中设定 `debug-assertions=yes` 时被保留下来，且该选项默认关闭。所以，如果看不见 `DEBUG` 日誌，尤其当你执行 `RUSTC_LOG=rustc rustc some.rs` 却只看见 `INFO` 日誌，请确保 config.toml 中 `debug-assertions=yes`。

我也认为，在某些情况下，只改变这个设定不会触發重编译。因此如果你在曾经建置过的状态下修改了该设置，你可能会想使用 `x.py clean` 强迫重编译。

<!-- While calls to `error!`, `warn!` and `info!` are included in every build of the compiler,
calls to `debug!` and `trace!` are only included in the program if
`debug-assertions=yes` is turned on in config.toml (it is
turned off by default), so if you don't see `DEBUG` logs, especially
if you run the compiler with `RUSTC_LOG=rustc rustc some.rs` and only see
`INFO` logs, make sure that `debug-assertions=yes` is turned on in your
config.toml.

I also think that in some cases just setting it will not trigger a rebuild,
so if you changed it and you already have a compiler built, you might
want to call `x.py clean` to force one. -->

### 日誌礼议和惯例
由于 `debug!` 默认会被移除，通常你不需担心「无谓地」呼叫 `debug!` 并把它们提交出去──它们不会拖累最终成品的效能，而且如果它们能帮助你定位一个臭虫，很可能也会帮助到其它人。

一个不算太严格的惯例是：在 `foo` 函式 *开始* 时使用 `debug!("foo(...)")`，在函式 *进行中* 时使用 `debug!("foo: ...")`。另一个小惯例是在调试日誌中使用 `{:?}`。

<!-- Because calls to `debug!` are removed by default, in most cases, don't worry
about adding "unnecessary" calls to `debug!` and leaving them in code you
commit - they won't slow down the performance of what we ship, and if they
helped you pinning down a bug, they will probably help someone else with a
different one.

A loosely followed convention is to use `debug!("foo(...)")` at the _start_ of
a function `foo` and `debug!("foo: ...")` _within_ the function. Another
loosely followed convention is to use the `{:?}` format specifier for debug
logs.

One thing to be **careful** of is **expensive** operations in logs.

If in the module `rustc::foo` you have a statement -->

一个 **需要注意** 的事项是日誌中的 **昂贵** 操作。

如果你在 `rustc::foo` 模块中有以下叙述：

```Rust
debug!("{:?}", random_operation(tcx));
```

则当别人用 `RUSTC_LOG=rustc::bar` 来执行 `rustc` 的调试建置，`random_operation` 将被执行。

这代表你不该把太昂贵或可能崩溃的操作放在日誌中──这会给所有想要在自己模块中使用日誌的人造成困扰。没人会發现它，直到有人试着用日誌来定位 *另一个* 臭虫。

<!-- Then if someone runs a debug `rustc` with `RUSTC_LOG=rustc::bar`, then
`random_operation()` will run.

This means that you should not put anything too expensive or likely to crash
there - that would annoy anyone who wants to use logging for their own module.
No-one will know it until someone tries to use logging to find *another* bug. -->

## 格式化 Graphviz 的输出(.dot 档案)
[formatting-graphviz-output]: #formatting-graphviz-output

某些编译器调适选项会产生 graphviz 图像──例如，`#[rustc_mir(borrowck_graphviz_postflow="suffix.dot")]` 特徵会输出多种 borrow-checker 的资料流图像。

这类选项都将生成 `.dot` 档案。欲检视此类档案，可以安装 graphviz （例如 `apt-get install graphviz`）并执行：

<!-- Some compiler options for debugging specific features yield graphviz graphs -
e.g. the `#[rustc_mir(borrowck_graphviz_postflow="suffix.dot")]` attribute
dumps various borrow-checker dataflow graphs.

These all produce `.dot` files. To view these files, install graphviz (e.g.
`apt-get install graphviz`) and then run the following commands: -->

```bash
$ dot -T pdf maybe_init_suffix.dot > maybe_init_suffix.pdf
$ firefox maybe_init_suffix.pdf # Or your favorite pdf viewer
```

## Narrowing (Bisecting) Regressions <!-- 不确定这标题怎麽翻… -->

[cargo-bisect-rustc][bisect] 工具可以快速并简单地找到是哪个 PR 导致了 `rustc` 的行为改变。它会自动下载 `rustc` PR 产出物，并在你给定的专案上进行测试，直到發现行为改变。你可以检视该 PR 以釐清 *为何* 它改变了。关于如何使用该工具，详见 [这份教学][bisect-tutorial]。

<!-- The [cargo-bisect-rustc][bisect] tool can be used as a quick and easy way to
find exactly which PR caused a change in `rustc` behavior. It automatically
downloads `rustc` PR artifacts and tests them against a project you provide
until it finds the regression. You can then look at the PR to get more context
on *why* it was changed.  See [this tutorial][bisect-tutorial] on how to use
it. -->

[bisect]: https://github.com/rust-lang-nursery/cargo-bisect-rustc
[bisect-tutorial]: https://github.com/rust-lang-nursery/cargo-bisect-rustc/blob/master/TUTORIAL.md

## 从 Rust 的 CI 下载产出物

kennytm 开發的 [rustup-toolchain-install-master][rtim] 工具可以从 Rust 的 CI 下载特定 SHA1 的产出物──这对应到某些 PR 能否成功落地──并为你在本地端使用做好准备。这也能用于 `@borstry` 的产出物。当你想要检视某个 PR 的建置成果、却不想自己建置时，这个工具非常有用。

The [rustup-toolchain-install-master][rtim] tool by kennytm can be used to
download the artifacts produced by Rust's CI for a specific SHA1 -- this
basically corresponds to the successful landing of some PR -- and then sets
them up for your local use. This also works for artifacts produced by `@bors
try`. This is helpful when you want to examine the resulting build of a PR
without doing the build yourself.

[rtim]: https://github.com/kennytm/rustup-toolchain-install-master
