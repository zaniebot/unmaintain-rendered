```yaml
number: 10325
title: "[`flake8-noqa`] Implement `NQA002`, `NQA003`, `NQA004`, and `NQA005`"
type: pull_request
state: closed
author: augustelalande
labels: []
assignees: []
base: main
head: noqa
created_at: 2024-03-11T06:07:56Z
updated_at: 2024-04-27T04:15:47Z
url: https://github.com/astral-sh/ruff/pull/10325
synced_at: 2026-01-10T22:37:01Z
```

# [`flake8-noqa`] Implement `NQA002`, `NQA003`, `NQA004`, and `NQA005`

---

_Pull request opened by @augustelalande on 2024-03-11 06:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `flake8-noqa` rules. Part of #850.

## Test Plan

Test fixtures have been added for all rules.


---

_Converted to draft by @augustelalande on 2024-03-11 06:08_

---

_Comment by @augustelalande on 2024-03-11 06:11_

I'm hoping someone can help me here. I've basically finalized the rules, but I can't run `cargo test` because the project won't compile as whole, as can be seen in the failing test runs. It seems to be related to the new plugin being added to the registry, but I can't put my finger on the issue.

Appreciate any help.

---

_Renamed from "[`flake8-noqa`] Implement `NQA002`, `NQA003`, `NQA004`, and `NQA005`" to "[`flake8-noqa`][help] Implement `NQA002`, `NQA003`, `NQA004`, and `NQA005`" by @augustelalande on 2024-03-11 06:11_

---

_Comment by @zanieb on 2024-03-12 19:56_

Hi @augustelalande thanks for the pull request! I haven't looked at the changes yet but I think what you're looking for is `cargo dev generate-all` to get the schema up to date with your changes.

---

_Comment by @augustelalande on 2024-03-12 20:05_

@zanieb cargo dev generate-all fails with the same error when compiling.
```
➜  cargo dev generate-all
   Compiling ruff_source_file v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_source_file)
   Compiling chrono v0.4.35
   Compiling ruff_formatter v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_formatter)
   Compiling lsp-types v0.95.0
   Compiling lsp-server v0.7.6
   Compiling ruff_python_trivia v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_trivia)
   Compiling ruff_notebook v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_notebook)
   Compiling ruff_python_ast v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_ast)
   Compiling quick-junit v0.3.5
   Compiling ruff_python_parser v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_parser)
   Compiling ruff_python_semantic v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_semantic)
   Compiling ruff_python_index v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_index)
   Compiling ruff_python_codegen v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_codegen)
   Compiling ruff_python_formatter v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_python_formatter)
   Compiling ruff_linter v0.3.2 (C:\Users\Auguste\workspace\ruff\crates\ruff_linter)
   Compiling ruff_workspace v0.0.0 (C:\Users\Auguste\workspace\ruff\crates\ruff_workspace)
error[E0275]: overflow evaluating the requirement `fn() -> RuleCodePrefix: Unpin`
  |
  = help: consider increasing the recursion limit by adding a `#![recursion_limit = "256"]` attribute to your crate (`ruff_linter`)
note: required because it appears within the type `PhantomData<fn() -> RuleCodePrefix>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\marker.rs:809:12
note: required because it appears within the type `std::iter::Empty<RuleCodePrefix>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\sources\empty.rs:31:12
note: required because it appears within the type `std::option::Option<std::iter::Empty<RuleCodePrefix>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Empty<RuleCodePrefix>, Map<AirflowIter, {closure@codes.rs:65:1}>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Empty<RuleCodePrefix>, Map<AirflowIter, ...>>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Empty<RuleCodePrefix>, Map<AirflowIter, ...>>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Empty<RuleCodePrefix>, Map<AirflowIter, ...>>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Empty<RuleCodePrefix>, Map<..., ...>>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Empty<RuleCodePrefix>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Empty<RuleCodePrefix>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Empty<...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<Empty<...>, ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Map<Chain<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\map.rs:62:12
note: required because it appears within the type `Option<Map<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<Map<Chain<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Option<Chain<Map<Chain<Chain<Chain<..., ...>, ...>, ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\option.rs:570:10
note: required because it appears within the type `Chain<IntoIter<String, 6>, Chain<Map<Chain<..., ...>, ...>, ...>>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\chain.rs:23:12
note: required because it appears within the type `Filter<Chain<IntoIter<String, 6>, Chain<Map<..., ...>, ...>>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\filter.rs:19:12
note: required because it appears within the type `Filter<Filter<Chain<IntoIter<String, 6>, Chain<..., ...>>, ...>, ...>`
 --> /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce\library\core\src\iter\adapters\filter.rs:19:12

For more information about this error, try `rustc --explain E0275`.
error: could not compile `ruff_linter` (lib) due to 1 previous error
warning: build failed, waiting for other jobs to finish...
```

---

_Comment by @zanieb on 2024-03-12 21:11_

Ah sorry I looked at the Linux CI which was failing for the reason I said but the Windows CI is indeed failing with that error

```
error[E0275]: overflow evaluating the requirement `fn() -> RuleCodePrefix: Unpin`
  |
  = help: consider increasing the recursion limit by adding a `#![recursion_limit = "256"]` attribute to your crate (`ruff_linter`)
```

I'd recommend trying the change they suggest here.

cc @konstin who knows ~a bit more about Windows-specific Rust problems~ lots more than me :)


---

_Comment by @augustelalande on 2024-03-12 22:22_

@zanieb It doesn't seem like a windows only issue. Check out the mkdocs check (running ubuntu).

---

_Comment by @zanieb on 2024-03-12 22:40_

Thanks I could reproduce locally.

https://github.com/astral-sh/ruff/pull/10325/commits/1ad9498f3b9628d389ec95e5fd4a477ca324c387 should resolve this, I pushed to your branch to unblock you and we'll consider it separately in #10373 

---

_Comment by @augustelalande on 2024-03-12 22:55_

@zanieb Thanks I was wary of making the change because I was also not sure of the implications.

---

_Renamed from "[`flake8-noqa`][help] Implement `NQA002`, `NQA003`, `NQA004`, and `NQA005`" to "[`flake8-noqa`] Implement `NQA002`, `NQA003`, `NQA004`, and `NQA005`" by @augustelalande on 2024-03-12 23:05_

---

_Comment by @github-actions[bot] on 2024-03-12 23:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/0068f885abfa158096a0d31147afe788e3724f45/disnake/utils.py#L1161'>disnake/utils.py:1161:48:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_ionization_collection.py#L461'>plasmapy/particles/tests/test_ionization_collection.py:461:40:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_ionization_collection.py#L477'>plasmapy/particles/tests/test_ionization_collection.py:477:40:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_ionization_collection.py#L494'>plasmapy/particles/tests/test_ionization_collection.py:494:40:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_particle_class.py#L557'>plasmapy/particles/tests/test_particle_class.py:557:51:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/utils/tests/test_code_repr.py#L327'>plasmapy/utils/tests/test_code_repr.py:327:41:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/0068f885abfa158096a0d31147afe788e3724f45/disnake/utils.py#L1161'>disnake/utils.py:1161:48:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/core/agent.py#L422'>rasa/core/agent.py:422:99:</a> RUF030 [*] `noqa` directives should have one space after the colon
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/tensorflow/model_data.py#L106'>rasa/utils/tensorflow/model_data.py:106:114:</a> RUF030 [*] `noqa` directives should have one space after the colon
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_ionization_collection.py#L461'>plasmapy/particles/tests/test_ionization_collection.py:461:40:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_ionization_collection.py#L477'>plasmapy/particles/tests/test_ionization_collection.py:477:40:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_ionization_collection.py#L494'>plasmapy/particles/tests/test_ionization_collection.py:494:40:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/particles/tests/test_particle_class.py#L557'>plasmapy/particles/tests/test_particle_class.py:557:51:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/d3281263b4fbae66807156fa34755c526840414f/plasmapy/utils/tests/test_code_repr.py#L327'>plasmapy/utils/tests/test_code_repr.py:327:41:</a> RUF100 [*] Unused `noqa` directive (duplicated: `S307`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/a67847caf4756dd07e3f42b25318117bbf767e2f/ibis/backends/tests/test_client.py#L1544'>ibis/backends/tests/test_client.py:1544:43:</a> RUF030 [*] `noqa` directives should have one space after the colon
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 6 | 6 | 0 | 0 | 0 |
| RUF030 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @augustelalande on 2024-03-12 23:32_

---

_Assigned to @MichaReiser by @zanieb on 2024-03-22 16:26_

---

_Comment by @MichaReiser on 2024-03-28 15:44_

Thank you @augustelalande, for working on the noqa rules. 

I've reviewed the rules and to my understanding:
* `NQA002`, `NQA003` overlap with [`PGH004`](https://docs.astral.sh/ruff/rules/blanket-noqa/): 
  * `x = 2  # noqa X100` gets flagged by `PGH004`, although the error message could be improved ([playground](https://play.ruff.rs/90ffb85e-26d8-43af-a2aa-e50b73fff73f))
  * `x = 2  # noqa  : X400` gets flagged by `PGH004`. Again, the error message could be improved ([playground](https://play.ruff.rs/fb99015b-afe4-4b39-9b8d-038f8671b4fa))
* `NQA005` overlaps with [`RUF100`](https://docs.astral.sh/ruff/rules/unused-noqa/), although `RUF100`. `RUF100` doesn't flag `x = 2  # noqa: D100, D100, D100` today but the rule is focused on identifying unused noqa directives (the explanation is more narrowly focused but I think it fits well from the rule name)

That's why I think that it would be best to integrate:
* `NQA002` and `NQA003` into `PGH004` by improving the error message and providing a specific fix when we identify the incorrect formatting fix. The rules fit in my view because `blanket-noqa` is about catching any noqa suppressions that ruff would interpret as blanket noqas
* `NQA005` into `RUF100`

I don't think there's an existing rule that covers `NQA004`, although I think it's worth considering moving the rule to `RUF`, considering that it's the only remaining noqa rule. 

What do you think?


---

_Comment by @augustelalande on 2024-03-28 17:45_

That sounds fine to me.

---

_Comment by @augustelalande on 2024-03-28 20:29_

Done. I also added a new RUF rule to detect missing spaces after noqa directives, e.g., `x = 2  # noqa:X100`. Which is the natural counterpart to `NQA004`.

If you merge this, you may want to consider closing #850, as I don't think anything else from there needs to be added to ruff.

---

_Comment by @augustelalande on 2024-04-09 22:17_

I'm gonna split this into seperate PRs to make it easier to merge.

---

_Closed by @augustelalande on 2024-04-09 23:52_

---

_Branch deleted on 2024-04-27 04:15_

---
