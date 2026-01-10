```yaml
number: 10469
title: "Fix `E231` bug: Inconsistent catch compared to pycodestyle, such as when dict nested in list"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: e231-nested-bug
created_at: 2024-03-19T04:49:34Z
updated_at: 2024-03-21T12:12:46Z
url: https://github.com/astral-sh/ruff/pull/10469
synced_at: 2026-01-10T22:47:02Z
```

# Fix `E231` bug: Inconsistent catch compared to pycodestyle, such as when dict nested in list

---

_Pull request opened by @augustelalande on 2024-03-19 04:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix `E231` bug: Inconsistent catch compared to pycodestyle, such as when dict nested in list. Resolves #10113.

## Test Plan

Example from #10113 added to test fixture.


---

_Comment by @github-actions[bot] on 2024-03-19 05:10_

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

_Label `bug` added by @MichaReiser on 2024-03-19 08:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:58 on 2024-03-20 13:48_

We should avoid initializing the stack with one element because it means that we need to allocate two vecs for every logical line, even if that line never has any parentheses. 

```suggestion
    let mut lsqb_stack = Vec::new();
    let mut lbrace_stack = Vec::new();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E231_E23.py.snap`:153 on 2024-03-20 13:52_

Could you add a few tests for when a list contains a dictionary? 

---

_@MichaReiser reviewed on 2024-03-20 13:52_

---

_Comment by @MichaReiser on 2024-03-20 13:52_

Thanks for looking into this. Using a stack here makes sense. I'm surprised that this isn't hitting performance more, considering that we're now allocating a new `Vec` for each logical line (actually two).  

---

_@augustelalande reviewed on 2024-03-20 18:33_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E231_E23.py.snap`:153 on 2024-03-20 18:33_

Done

---

_Comment by @augustelalande on 2024-03-20 18:37_

> Thanks for looking into this. Using a stack here makes sense. I'm surprised that this isn't hitting performance more, considering that we're now allocating a new `Vec` for each logical line (actually two).

Ya it's surprising. But is allocating a new `Vec` really significantly more expensive than allocating any other variable, if it's not populated?

---

_Comment by @augustelalande on 2024-03-21 05:05_

Took inspiration from `extraneous_whitespace.rs` and reduced it to one stack

---

_Comment by @MichaReiser on 2024-03-21 08:12_

> Ya it's surprising. But is allocating a new Vec really significantly more expensive than allocating any other variable, if it's not populated?

That's correct: Creating an empty `Vec` is "free" because it doesn't allocate. But a `Vec` is different from e.g. a `TextSize` in that it allocates memory on the heap when you push the first element. That's different from a `TextSize`  variable that is always stored on the stack (which is as cheap as memory can be)

> Took inspiration from extraneous_whitespace.rs and reduced it to one stack

Nice, thank you

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:100 on 2024-03-21 08:13_

I find this much easier to understand. Nice refactor

---

_@MichaReiser approved on 2024-03-21 08:13_

---

_Merged by @MichaReiser on 2024-03-21 08:13_

---

_Closed by @MichaReiser on 2024-03-21 08:13_

---

_Branch deleted on 2024-03-21 12:12_

---
