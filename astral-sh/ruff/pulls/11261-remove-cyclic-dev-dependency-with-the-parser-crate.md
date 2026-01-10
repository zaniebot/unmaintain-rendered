```yaml
number: 11261
title: Remove cyclic dev dependency with the parser crate
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/resolve-ast-parser-dev-cycle-dep
created_at: 2024-05-03T08:52:01Z
updated_at: 2024-05-08T22:49:30Z
url: https://github.com/astral-sh/ruff/pull/11261
synced_at: 2026-01-10T22:05:26Z
```

# Remove cyclic dev dependency with the parser crate

---

_Pull request opened by @dhruvmanila on 2024-05-03 08:52_

## Summary

This PR removes the cyclic dev dependency some of the crates had with the parser crate.

The cyclic dependencies are:
* `ruff_python_ast` has a **dev dependency** on `ruff_python_parser` and `ruff_python_parser` directly depends on `ruff_python_ast`
* `ruff_python_trivia` has a **dev dependency** on `ruff_python_parser` and `ruff_python_parser` has an indirect dependency on `ruff_python_trivia` (`ruff_python_parser` - `ruff_python_ast` - `ruff_python_trivia`)

Specifically, this PR does the following:
* Introduce two new crates
	* `ruff_python_ast_integration_tests` and move the tests from the `ruff_python_ast` crate which uses the parser in this crate
	* `ruff_python_trivia_integration_tests` and move the tests from the `ruff_python_trivia` crate which uses the parser in this crate

### Motivation

The main motivation for this PR is to help development. Before this PR, `rust-analyzer` wouldn't provide any intellisense in the `ruff_python_parser` crate regarding the symbols in `ruff_python_ast` crate.

```
[ERROR][2024-05-03 13:47:06] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/dhruv/.cargo/bin/rust-analyzer"	"stderr"	"[ERROR project_model::workspace] cyclic deps: ruff_python_parser(Idx::<CrateData>(50)) -> ruff_python_ast(Idx::<CrateData>(37)), alternative path: ruff_python_ast(Idx::<CrateData>(37)) -> ruff_python_parser(Idx::<CrateData>(50))\n"
```

## Test Plan

Check the logs of `rust-analyzer` to not see any signs of cyclic dependency.

---

_Label `internal` added by @dhruvmanila on 2024-05-03 08:52_

---

_Marked ready for review by @dhruvmanila on 2024-05-03 09:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-03 09:00_

---

_Comment by @github-actions[bot] on 2024-05-03 09:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-05-03 12:03_

Hmmm, I struggle with

> Move all of the test cases that depends on the parser from ruff_python_ast to ruff_python_parser

The problem is that the tests are now no longer living next to the implementations. This makes them hard to discover or even knowing why a test failed (My intuition for a failing test in the parser crate is that the parser is incorrect). 

---

_Comment by @charliermarsh on 2024-05-03 13:24_

Removing the cyclic dep is more important to me than co-locating the tests, so I’d still vote in favor of this.

---

_Comment by @charliermarsh on 2024-05-03 13:25_

The other alternative is to create a third crate that depends on both, and put the tests in there, but that will also bring some confusion.

---

_Comment by @MichaReiser on 2024-05-03 14:45_

> Removing the cyclic dep is more important to me than co-locating the tests, so I’d still vote in favor of this.

Because of rust-analyzer? Because it isn't a cyclic dependency according to rust

* ruff_python_ast
* ruff_python_parser: depends on ruff_python_ast
* ruff_python_ast (tests): depends on ruff_python_parser

---

_Comment by @charliermarsh on 2024-05-03 16:03_

Yeah, because of rust-analyzer. It’s causing real problems for people!

---

_Comment by @MichaReiser on 2024-05-03 16:15_

Yeah. A few alternative options where I don't have a strong preference:

* Create a subfolder in the parser crate for the `ast` tests, e.g. `tests/ast` to make it clear what this tests are about
* Create a sub-crate in the `ruff_python_ast/integration_tests` named `ruff_python_ast_integration_tests` and add it to the workspace members in the root `Cargo.toml` 

I'm slightly preferring the second option. I don't know if @BurntSushi has any other recommendation on how we could restructure our project to avoid the rust-analyzer cyclic dependency.

---

_Comment by @BurntSushi on 2024-05-03 16:50_

It seems like this is the rust-analyzer issue: https://github.com/rust-lang/rust-analyzer/issues/3390

I guess it's a problem because `rust-analyzer` just always enables `cfg(test)`. Which makes sense. That's what you want. But when the test library introduces a circular dependency, that turns into a true circular dependency if `cfg(test)` is always unconditionally enabled.

I don't really know another way to disable it. I ran into this problem too, but didn't diagnose the root cause. I'd definitely vote in favor of fixing this somehow. It'd be nice if RA could figure out how to make this work though, because this is a legitimate pattern in the ecosystem.

---

_Comment by @charliermarsh on 2024-05-03 17:02_

Yeah, weak vote for making a separate crate. (Personally fine with it being at `crates/ruff_python_ast_integration_tests` for simplicity.)

---

_Comment by @dhruvmanila on 2024-05-06 10:53_

I've updated the PR description to reflect the recent changes. Thank you all for the recommendations.

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-07 03:31_

---

_@charliermarsh approved on 2024-05-07 03:45_

---

_@MichaReiser approved on 2024-05-07 05:57_

Thanks!

You may want to add a readme to both crates shortly explaining why they exist (link to the rust analyzer issue). It could prevent that someone undoes the change in the future

---

_Merged by @dhruvmanila on 2024-05-07 09:24_

---

_Closed by @dhruvmanila on 2024-05-07 09:24_

---

_Branch deleted on 2024-05-07 09:24_

---

_Comment by @Veykril on 2024-05-08 22:49_

> It seems like this is the rust-analyzer issue: [rust-lang/rust-analyzer#3390](https://github.com/rust-lang/rust-analyzer/issues/3390)

Stumbled upon this just now, there is an issue on the rust-analyzer repo with some more info about this than the one you linked here https://github.com/rust-lang/rust-analyzer/issues/14167

---
