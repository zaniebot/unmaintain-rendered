```yaml
number: 10419
title: "[`flake8_comprehensions`] Handled special case for `C400` which also matches `C416`"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - rule
assignees: []
merged: true
base: main
head: unnecessary-generator-list
created_at: 2024-03-15T08:11:09Z
updated_at: 2024-03-15T14:39:26Z
url: https://github.com/astral-sh/ruff/pull/10419
synced_at: 2026-01-12T15:55:32Z
```

# [`flake8_comprehensions`] Handled special case for `C400` which also matches `C416`

---

_@hikaru-kajita_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Short-circuit implementation mentioned in #10403.

I implemented this by extending C400:
- Made `UnnecessaryGeneratorList` have information of whether the the short-circuiting occurred (to put diagnostic)
- Add additional check for whether in `unnecessary_generator_list` function.

Please give me suggestions if you think this isn't the best way to handle this :)

## Test Plan

<!-- How was it tested? -->

Extended `C400.py` a little, and written the cases where:
- Code could be converted to one single conversion to `list` e.g. `list(x for x in range(3))` -> `list(range(3))`
- Code couldn't be converted to one single conversion to `list` e.g. `list(2 * x for x in range(3))` -> `[2 * x for x in range(3)]`
- `list` function is not built-in, and should not modify the code in any way.

---

_Comment by @github-actions[bot] on 2024-03-15 08:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-15 13:56_

---

_Label `rule` added by @charliermarsh on 2024-03-15 13:56_

---

_@charliermarsh approved on 2024-03-15 14:21_

Awesome, thank you!

---

_@charliermarsh reviewed on 2024-03-15 14:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs`:104 on 2024-03-15 14:21_

I decided to just use more nesting here rather than the `break` pattern. It's not necessarily "better", just a matter of personal preference.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs`:80 on 2024-03-15 14:21_

`.as_generator_expr()` lets us avoid the clone.

---

_@charliermarsh reviewed on 2024-03-15 14:21_

---

_Merged by @charliermarsh on 2024-03-15 14:34_

---

_Closed by @charliermarsh on 2024-03-15 14:34_

---
