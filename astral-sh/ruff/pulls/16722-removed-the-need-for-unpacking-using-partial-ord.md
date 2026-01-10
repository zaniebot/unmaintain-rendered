```yaml
number: 16722
title: removed the need for unpacking using partial ord derive
type: pull_request
state: closed
author: ekohilas
labels: []
assignees: []
base: main
head: num-ordering-cleanup
created_at: 2025-03-14T08:50:44Z
updated_at: 2025-03-14T09:29:28Z
url: https://github.com/astral-sh/ruff/pull/16722
synced_at: 2026-01-10T19:49:02Z
```

# removed the need for unpacking using partial ord derive

---

_Pull request opened by @ekohilas on 2025-03-14 08:50_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Uses `PartialOrd` derive on Number to allow for cleaner comparisons in rules such as in `long_sleep_not_forever`

## Test Plan

Run on existing tests


---

_@MichaReiser reviewed on 2025-03-14 08:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1894 on 2025-03-14 08:52_

I don't think deriving `PartialOrd` on `Number` is correct because `Float(10.123)` is now larger than `Int(10000)`

---

_Comment by @github-actions[bot] on 2025-03-14 08:59_

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

_@ekohilas reviewed on 2025-03-14 09:00_

---

_Review comment by @ekohilas on `crates/ruff_python_ast/src/nodes.rs`:1894 on 2025-03-14 09:00_

I was just thinking about something like that, however I don't quite understand the example given. I understand `Float` to be stored using `f64` and `Int` partially using `u64`?, which then rust would compare instead?

---

_@MichaReiser reviewed on 2025-03-14 09:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1894 on 2025-03-14 09:03_

The implementation derived by `PartialEq`:

* If both left and right are of the same variant: compare the inner values
* Otherwise: compare the variant index only

---

_Closed by @MichaReiser on 2025-03-14 09:13_

---

_@ekohilas reviewed on 2025-03-14 09:18_

---

_Review comment by @ekohilas on `crates/ruff_python_ast/src/nodes.rs`:1894 on 2025-03-14 09:18_

Right I just had another look at the docs thanks!

I'll see if I can manually define `PartialOrd` on `Number`, unless you had other suggestions?

---

_@MichaReiser reviewed on 2025-03-14 09:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1894 on 2025-03-14 09:29_

The problem is that `Number` can be a `str`, which makes comparisons extremely awkward. I guess we could return `None` in that case (unless they're `Equal`). However, I'm not convinced that we want to make comparing `Number` easy. I think it's good that callers have to think through those cases. 

---
