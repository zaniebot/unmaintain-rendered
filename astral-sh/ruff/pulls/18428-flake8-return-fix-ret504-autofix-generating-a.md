```yaml
number: 18428
title: "[`flake8-return`] Fix `RET504` autofix generating a syntax error"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-RET504
created_at: 2025-06-02T14:06:52Z
updated_at: 2025-06-11T13:42:42Z
url: https://github.com/astral-sh/ruff/pull/18428
synced_at: 2026-01-12T15:56:18Z
```

# [`flake8-return`] Fix `RET504` autofix generating a syntax error

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18411

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression test.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-02 14:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:620 on 2025-06-10 11:05_

This now produces a syntax error for:

```py
def f():
    x = (1
              )
    return x
```

---

_@MichaReiser reviewed on 2025-06-10 11:05_

---

_@MichaReiser requested changes on 2025-06-10 11:06_

This can now result in a different syntax error

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-10 13:44_

---

_@MichaReiser reviewed on 2025-06-11 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:618 on 2025-06-11 06:45_

Iterating over all tokens is unnecessarily slow. Especially given that you know exactly what token you're looking for. You can use `checker.tokens().in_range(assign.range()`  or `after(assign.start)` instead

---

_@MichaReiser reviewed on 2025-06-11 06:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:626 on 2025-06-11 06:46_

```suggestion
                if content[eq_token.end().to_usize()..]
```

---

_@MichaReiser reviewed on 2025-06-11 06:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:618 on 2025-06-11 06:50_

I think the safest here is to use `before` and take all tokens up to the start of the assignment value. Then use `rfind` to find the last equal sign

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-11 13:35_

---

_Label `bug` added by @MichaReiser on 2025-06-11 13:38_

---

_Label `fixes` added by @MichaReiser on 2025-06-11 13:38_

---

_@MichaReiser approved on 2025-06-11 13:38_

Thank you

---

_Merged by @MichaReiser on 2025-06-11 13:38_

---

_Closed by @MichaReiser on 2025-06-11 13:38_

---
