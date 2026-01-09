---
number: 15397
title: "[red-knot] Move type-property tests to Markdown"
type: issue
state: closed
author: sharkdp
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-01-10T11:15:05Z
updated_at: 2025-01-20T07:42:39Z
url: https://github.com/astral-sh/ruff/issues/15397
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Move type-property tests to Markdown

---

_Issue opened by @sharkdp on 2025-01-10 11:15_

The various type-property tests here …

https://github.com/astral-sh/ruff/blob/b861551b6ac928c25136d76151162f6fefc9cf71/crates/red_knot_python_semantic/src/types.rs#L4155-L4673

… can now be moved to Markdown-based tests which are more concise, easier to read, can be run without recompilation, and can contain additional prose for documentation.

An example on how to do this can be seen in [this PR](https://github.com/astral-sh/ruff/pull/15353) which moved the `is_assignable_to` tests (ignore the changes in `property_tests.rs`).

If you would like to work on this, let us know and pick *one* of these:

- [x] `tuple_containing_never_simplifies_to_never`
- [x] `is_assignable_to`
- [x] `is_subtype_of`
- [x] `is_equivalent_to`
- [x] `is_disjoint_from`
- [x] `is_singleton`
- [x] `is_single_valued`
- [x] `is_fully_static`
- [x] truthiness

---

_Label `help wanted` added by @sharkdp on 2025-01-10 11:15_

---

_Label `red-knot` added by @sharkdp on 2025-01-10 11:15_

---

_Comment by @InSyncWithFoo on 2025-01-10 12:12_

> [...] and pick *one* of these

Why only one? Wouldn't it be easier for both the contributor and the reviewers to have one instead of multiple PRs? These are closely related, after all.

---

_Comment by @sharkdp on 2025-01-10 13:02_

They are related tasks of course, but they can be cleanly separated. The Rust code in these tests changes from time to time (the latest refactoring that changed these was yesterday), and new test cases are added. So if we merge all of these into one giant contribution, it increases the risk for annoying merge conflicts that could be avoided otherwise. As a maintainer, I also prefer multiple smaller contributions. That said, obviously I won't complain if someone wants to tackle more or all of these at once.

---

_Comment by @AlexWaygood on 2025-01-10 13:42_

I strongly agree that small, incremental PRs are much easier to review, and therefore much more helpful

---

_Referenced in [astral-sh/ruff#15402](../../astral-sh/ruff/pulls/15402.md) on 2025-01-10 14:26_

---

_Referenced in [astral-sh/ruff#15437](../../astral-sh/ruff/pulls/15437.md) on 2025-01-12 00:12_

---

_Referenced in [astral-sh/ruff#15469](../../astral-sh/ruff/pulls/15469.md) on 2025-01-14 09:34_

---

_Referenced in [astral-sh/ruff#15470](../../astral-sh/ruff/pulls/15470.md) on 2025-01-14 09:36_

---

_Referenced in [astral-sh/ruff#15533](../../astral-sh/ruff/pulls/15533.md) on 2025-01-16 14:59_

---

_Referenced in [astral-sh/ruff#15534](../../astral-sh/ruff/pulls/15534.md) on 2025-01-16 16:01_

---

_Referenced in [astral-sh/ruff#15563](../../astral-sh/ruff/pulls/15563.md) on 2025-01-18 00:10_

---

_Referenced in [astral-sh/ruff#15580](../../astral-sh/ruff/pulls/15580.md) on 2025-01-19 00:21_

---

_Closed by @sharkdp on 2025-01-20 07:42_

---

_Closed by @sharkdp on 2025-01-20 07:42_

---

_Comment by @sharkdp on 2025-01-20 07:42_

Thank you @InSyncWithFoo 

---
