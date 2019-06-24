<!--
# About the compiler team

rustc is maintained by the [Rust compiler team][team]. The people who belong to
this team collectively work to track regressions and implement new features.
Members of the Rust compiler team are people who have made significant
contributions to rustc and its design.

[team]: https://www.rust-lang.org/governance/teams/language-and-compiler
-->

# 关于编译器团队

rustc 由 [Rust 编译器团队][团队] 维护。这个团队的成员共同跟踪性能退化问题、实现新特性。
Rust 编译器团队的成员由为 rustc 和它的设计做出重大贡献的人们组成。

[团队]: https://www.rust-lang.org/governance/teams/language-and-compiler

<!--
## Discussion

Currently the compiler team chats in 2 places:

- The `t-compiler` stream on [the Zulip instance][zulip]
- The `compiler` channel on the [rust-lang discord](https://discord.gg/rust-lang)
-->

## 讨论

目前编译器团队在两个地方讨论：

- [Zulip 实例][zulip] 的 `t-compiler` 流
- [rust-lang discord](https://discord.gg/rust-lang) 的 `compiler` 频道

<!--
## Expert map

If you're interested in figuring out who can answer questions about a
particular part of the compiler, or you'd just like to know who works on what,
check out our [experts directory](https://github.com/rust-lang/compiler-team/blob/master/experts/MAP.md).
It contains a listing of the various parts of the compiler and a list of people
who are experts on each one.
-->

## 专家地图

如果您有兴趣搞清楚谁可以回答关于编译器某个特定领域的问题，或者您只是想知道谁在做这方面的工作，
来看看我们的 [专家目录](https://github.com/rust-lang/compiler-team/blob/master/experts/MAP.md)。
它包含了一份编译器各部分的列表，以及每个部分的专家列表。

<!--
## Rust compiler meeting

The compiler team has a weekly meeting where we do triage and try to
generally stay on top of new bugs, regressions, and other things. This
general plan for this meeting can be found in
[the rust-compiler-meeting etherpad][etherpad]. It works roughly as
follows:

- **Review P-high bugs:** P-high bugs are those that are sufficiently
  important for us to actively track progress. P-high bugs should
  ideally always have an assignee.
- **Look over new regressions:** we then look for new cases where the
  compiler broke previously working code in the wild. Regressions are
  almost always marked as P-high; the major exception would be bug
  fixes (though even there we often
  [aim to give warnings first][procedure]).
- **Check I-nominated issues:** These are issues where feedback from
  the team is desired.
- **Check for beta nominations:** These are nominations of things to
  backport to beta.

The meeting currently takes place on Thursdays at 10am Boston time
(UTC-4 typically, but daylight savings time sometimes makes things
complicated).

The meeting is held over a "chat medium", currently on [zulip].

[etherpad]: https://public.etherpad-mozilla.org/p/rust-compiler-meeting
[procedure]: https://forge.rust-lang.org/rustc-bug-fix-procedure.html
[zulip]: https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler
-->

## Rust 编译器会议

编译器团队每周举办一个周会，我们在会上对问题进行分类，并聚焦在新的 bugs、性能退化和其它一些问题。
会议的总体计划在[rust 编译器会议 etherpad][etherpad]。大致如下：

- **评审 P-high bugs：** P-high bugs 对我们非常重要，需要积极跟踪进度。
  在理想情况下，P-high bugs 总是要分配给一个受托人。
- **检查新的性能退化：** 我们接着找出编译器破坏原本可以正常工作代码的案例。
  性能退化几乎总是被标记为 P-high；主要的例外是修复 bug（尽管我们经常会[首先给出警告][procedure]）
- **检查 I-nominated 问题：** 这些问题需要来自团队的反馈。
- **检查 beta-nominations：** 这些是被提名回退到 beta 状态的事情。

会议目前在波士顿时间每周二上午10点举行（通常是UTC-4时区，但是夏时制有时会带来混乱）。

会议通过“聊天媒体”举行，目前在[zulip]

[etherpad]: https://public.etherpad-mozilla.org/p/rust-compiler-meeting
[procedure]: https://forge.rust-lang.org/rustc-bug-fix-procedure.html
[zulip]: https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler

<!--
## Team membership

Membership in the Rust team is typically offered when someone has been
making significant contributions to the compiler for some
time. Membership is both a recognition but also an obligation:
compiler team members are generally expected to help with upkeep as
well as doing reviews and other work.

If you are interested in becoming a compiler team member, the first
thing to do is to start fixing some bugs, or get involved in a working
group. One good way to find bugs is to look for
[open issues tagged with E-easy](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AE-easy)
or
[E-mentor](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AE-mentor).
-->

## 团队成员资格

Rust 团队的成员资格通常提供给长期为编译器做出重大贡献的人。
成员资格既是一种承认，也是一种义务：
编译器团队成员通常需要帮助进行日常维护，以及进行评审和其它工作。

如果您有兴趣成为编译器团队成员，首先从修复 bug 开始，或者参与一个工作组的工作。
发现 bug 的一个好方法是寻找[使用 E-easy 标签的开放问题](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AE-easy)
或者[E-mentor 标签](https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AE-mentor).

<!--
### r+ rights

Once you have made a number of individual PRs to rustc, we will often
offer r+ privileges. This means that you have the right to instruct
"bors" (the robot that manages which PRs get landed into rustc) to
merge a PR
([here are some instructions for how to talk to bors][homu-guide]).

[homu-guide]: https://buildbot2.rust-lang.org/homu/

The guidelines for reviewers are as follows:

- You are always welcome to review any PR, regardless of who it is
  assigned to.  However, do not r+ PRs unless:
  - You are confident in that part of the code.
  - You are confident that nobody else wants to review it first.
    - For example, sometimes people will express a desire to review a
      PR before it lands, perhaps because it touches a particularly
      sensitive part of the code.
- Always be polite when reviewing: you are a representative of the
  Rust project, so it is expected that you will go above and beyond
  when it comes to the [Code of Conduct].

[Code of Conduct]: https://www.rust-lang.org/policies/code-of-conduct
-->

### r+ 权限

一旦您为 rustc 提交了一些独立的 PRs，我们通常会给您授予 r+ 特权。
这意味着您有权力指导“bors”（管理哪些 PR 合并进入 rustc 的机器人）合并一个 PR 
（[这里是如何与 bors 对话的一些指令][homu-guide]）

[homu-guide]: https://buildbot2.rust-lang.org/homu/

评审人的指导方针如下：

- 欢迎您随时查看任何 PR，不论它被指派给谁。然而，不包括 r+ 的 PR，除非：
  - 您对那部分代码有自信。
  - 您确信没有人想要首先评审它。
    - 例如，人们有时会表达在合并一个 PR 之前评审的愿望，
      可能是因为它和代码中特别敏感的部分有关。
- 在评审的时候始终保持礼貌：您是 Rust 项目的代表，
  因此，当涉及到[行为准则]时，人们期望您能做得更好。

[行为准则]： https://www.rust-lang.org/policies/code-of-conduct

<!--
### high-five

Once you have r+ rights, you can also be added to the high-five
rotation. high-five is the bot that assigns incoming PRs to
reviewers. If you are added, you will be randomly selected to review
PRs. If you find you are assigned a PR that you don't feel comfortable
reviewing, you can also leave a comment like `r? @so-and-so` to assign
to someone else — if you don't know who to request, just write `r?
@nikomatsakis for reassignment` and @nikomatsakis will pick someone
for you.

[hi5]: https://github.com/rust-highfive

Getting on the high-five list is much appreciated as it lowers the
review burden for all of us! However, if you don't have time to give
people timely feedback on their PRs, it may be better that you don't
get on the list.
-->

### high-five

一旦您拥有了 r+ 权限，您也可以被加入到 [high-five][hi5] 的自动流程中。
high-five 是负责分配新的 PR 给评审人的机器人。
如果您已经加入，您将会被随机抽选为评审人。
如果您觉得分配给您的 PR 您不喜欢评审，您可以留下一个评论 `r? @某人` 来把它分配给别人 -
如果您不知道请求谁，就写 `r? @nikomatsakis 来重新分配`，@nikomatsakis 会为您指派一个人。

[hi5]: https://github.com/rust-highfive

进入 high-five 名单是非常受欢迎的，因为它降低了我们所有人的评审门槛！
然而，如果您没有时间对人们的 PR 进行及时的反馈，也许您不出现在名单上更好。

<!--
### Full team membership

Full team membership is typically extended once someone made many
contributions to the Rust compiler over time, ideally (but not
necessarily) to multiple areas. Sometimes this might be implementing a
new feature, but it is also important — perhaps more important! — to
have time and willingness to help out with general upkeep such as
bugfixes, tracking regressions, and other less glamorous work.
-->

### 完整的团队成员资格

一旦有人长期为 Rust 编译器做出很多贡献，他通常会扩展到完整的团队成员资格，
理想情况（但不一定）是多个领域。有时，这可能是实现一个新特性，
但同样很重要 - 也许更重要的是 - 有时间和意愿帮助进行日常维护，
比如修复 bug，追踪性能退化，和其它不那么有吸引力的工作。

