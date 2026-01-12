```yaml
number: 10321
title: "[`flake8-unused-arguments`] Add fixes for `ARG001`->`ARG005`"
type: pull_request
state: closed
author: GtrMo
labels: []
assignees: []
base: main
head: new_fix_unused_parameters
created_at: 2024-03-10T17:11:46Z
updated_at: 2025-04-28T07:01:00Z
url: https://github.com/astral-sh/ruff/pull/10321
synced_at: 2026-01-12T15:55:31Z
```

# [`flake8-unused-arguments`] Add fixes for `ARG001`->`ARG005`

---

_@GtrMo_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds fixes for `unused-arguments` rules.
The fix removes the unused arguments from the function/method definition.
The fix is marked as unsafe as removing a parameter changes the function definition, and we cannot access call sites to also remove the argument there.

The `remove_parameter` function, analog to `remove_argument` was added. It is tested through the fix.

The rule was a bit refactored: the `function`, `method` and `call` functions were merged in a single `check` function


## Test Plan

New test cases were added to test with all parameter kinds.


---

_Comment by @codspeed-hq[bot] on 2024-03-10 17:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/GtrMo:new_fix_unused_parameters)

### Merging #10321 will **not alter performance**

<sub>Comparing <code>GtrMo:new_fix_unused_parameters</code> (ec08118) with <code>main</code> (8619986)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-03-10 17:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-06-08 20:55_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/fix/edits.rs`:136 on 2024-06-08 20:55_

How does this differ from `remove_argument`?

---

_Closed by @MichaReiser on 2025-04-28 07:01_

---
