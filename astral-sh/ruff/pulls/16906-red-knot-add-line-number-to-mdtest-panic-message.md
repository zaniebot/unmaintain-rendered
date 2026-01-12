```yaml
number: 16906
title: "[red-knot] Add line number to mdtest panic message about language tag mismatch"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: add-line-number-to-mdtest-lang-panic
created_at: 2025-03-21T22:04:22Z
updated_at: 2025-03-22T12:21:19Z
url: https://github.com/astral-sh/ruff/pull/16906
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Add line number to mdtest panic message about language tag mismatch

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #16898 

## Test Plan

Update test for lang mismatch panic


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-21 22:04_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-03-21 22:04_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-21 22:04_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-21 22:04_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-21 22:04_

---

_Comment by @github-actions[bot] on 2025-03-21 22:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Converted to draft by @MatthewMckee4 on 2025-03-21 22:20_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-21 23:19_

---

_Marked ready for review by @MatthewMckee4 on 2025-03-22 00:03_

---

_Comment by @MatthewMckee4 on 2025-03-22 00:24_

Need to change this slightly

---

_@MichaReiser reviewed on 2025-03-22 09:57_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:757 on 2025-03-22 09:57_

This shouldn't return a `TextSize`. We only use `TextSize` for byte offsets. What I understand this method does is that it returns the number of lines. It should use `OneIndexed` or `usize`)

---

_Review comment by @MatthewMckee4 on `crates/red_knot_test/src/parser.rs`:757 on 2025-03-22 11:36_

count_lines returns u32, i assume this would be okay?

---

_@MatthewMckee4 reviewed on 2025-03-22 11:36_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-03-22 11:48_

---

_@MichaReiser reviewed on 2025-03-22 12:04_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:757 on 2025-03-22 12:04_

Yes, a `u32` works too. Anything other than `TextSize`

---

_@MichaReiser approved on 2025-03-22 12:05_

---

_Merged by @MichaReiser on 2025-03-22 12:05_

---

_Closed by @MichaReiser on 2025-03-22 12:05_

---

_Branch deleted on 2025-03-22 12:21_

---
