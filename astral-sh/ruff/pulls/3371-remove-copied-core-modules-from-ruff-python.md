```yaml
number: 3371
title: "Remove copied `core` modules from `ruff_python_formatter`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ruff_python_ast_formatter
created_at: 2023-03-06T23:13:49Z
updated_at: 2023-03-08T19:03:43Z
url: https://github.com/astral-sh/ruff/pull/3371
synced_at: 2026-01-12T04:39:44Z
```

# Remove copied `core` modules from `ruff_python_formatter`

---

_Pull request opened by @charliermarsh on 2023-03-06 23:13_

## Summary

Previously, to save time, I copied some code into `ruff_python_formatter` to avoid creating a dependency on Ruff.

As of #3370, that code now lives in a standalone crate, upon which `ruff_python_formatter` can depend. This allows us to remove a couple of files that were largely duplicated, but had diverged in minor ways (e.g., `Locator` needs to be able to provide a ref-counted string).

## Test Plan

- `cargo clippy`
- `cargo test --all --all-features`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-06 23:13_

---

_Review requested from @konstin by @charliermarsh on 2023-03-06 23:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/locator.rs`:11 on 2023-03-07 09:37_

Adding an `Rc` means every `Locator` now performs a heap allocation. Is this used for getting "cheap" string formatting? Could we store a `Rc` source code on the formatting context instead (only paying once for the heap allocation)?

What's the difference of `contents` and `contents_rc`? Do we need both? 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/strings.rs`:3 on 2023-03-07 09:37_

Nit: When do you prefer full qualified paths vs importing them at the top?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/format/expr.rs`:41 on 2023-03-07 09:39_

Nit: I prefer using `Range::from(expr)` for its explicitness (easier to review without type hints of an IDE)

---

_@MichaReiser reviewed on 2023-03-07 09:40_

Does this PR seem contain more changes than just removing modules? Any chance we could split some of these reactors into smaller PRs?

---

_Review comment by @konstin on `crates/ruff_python_ast/src/source_code/locator.rs`:107 on 2023-03-07 10:07_

This copies the entire contents, right? i'm trying to understand how lifetimes and allocations work here

---

_Review comment by @konstin on `crates/ruff_python_ast/src/strings.rs`:58 on 2023-03-07 10:17_

`..` ranges are exclusive, so this doesn't test all combinations (https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=6eaf7a38144d10efe13d3455937b82ef). i'd just do `prefixes.enumerate()` twice here

---

_@konstin approved on 2023-03-07 10:18_

---

_Comment by @charliermarsh on 2023-03-07 15:42_

> Does this PR seem contain more changes than just removing modules? Any chance we could split some of these reactors into smaller PRs?

Yeah removing `Locator` meant using the _existing_ `Locator` which had a slightly different API. I can carve some of this out into separate PRs.

---

_@charliermarsh reviewed on 2023-03-07 15:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/locator.rs`:11 on 2023-03-07 15:44_

Let me try putting this on the formatting context instead... We already only create one `Locator` per file, so in practice I believe it'd be "the same", but it'd be nice to keep this separate from the `Locator` API itself.

---

_@charliermarsh reviewed on 2023-03-08 00:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/strings.rs`:3 on 2023-03-08 00:48_

(Moved to #3396, but I'll answer here that in this case, I'm using fully-qualified because `ruff_python_stdlib::str` and `ruff_python_stdlib::bytes` have identical member names.)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/strings.rs`:58 on 2023-03-08 00:48_

(Moved to #3396.)

---

_@charliermarsh reviewed on 2023-03-08 00:48_

---

_@charliermarsh reviewed on 2023-03-08 00:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/context.rs`:8 on 2023-03-08 00:57_

`Rc` was moved onto the formatter context.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/core/helpers.rs`:24 on 2023-03-08 00:58_

The `Locator` in `ruff_python_ast` has a slightly different API than the version we "forked" into here, hence these changes, e.g., we need to use a slightly different interface for slices.

---

_@charliermarsh reviewed on 2023-03-08 00:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/core/helpers.rs`:50 on 2023-03-08 00:58_

`index` got renamed to `offset` (`index` is also a field on the `Locator` struct -- this method didn't exist at all in the "upstream" version of `Locator` from `ruff_python_ast`).

---

_@charliermarsh reviewed on 2023-03-08 00:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/format/expr.rs`:41 on 2023-03-08 00:59_

I guess same question applies as in #3377: do we prefer `.into()`, `Range::from`, or making `Literal` generic on `Into<Range>`?

---

_@charliermarsh reviewed on 2023-03-08 00:59_

---

_@charliermarsh reviewed on 2023-03-08 03:15_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/format/expr.rs`:41 on 2023-03-08 03:15_

`Range::from` or the generic `Into<Range>` both seem reasonable to me, I defer to whatever feels more conventional.

---

_Merged by @charliermarsh on 2023-03-08 19:03_

---

_Closed by @charliermarsh on 2023-03-08 19:03_

---

_Branch deleted on 2023-03-08 19:03_

---
