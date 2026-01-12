```yaml
number: 7224
title: "Ignore `@override` method when enforcing `bad-dunder-name` rule"
type: pull_request
state: merged
author: brendonh8
labels:
  - bug
assignees: []
merged: true
base: main
head: plw3201-ignore-override
created_at: 2023-09-07T16:13:49Z
updated_at: 2023-09-12T12:00:49Z
url: https://github.com/astral-sh/ruff/pull/7224
synced_at: 2026-01-12T15:55:23Z
```

# Ignore `@override` method when enforcing `bad-dunder-name` rule

---

_@brendonh8_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
closes #6958 

If a method has the `override` decorator, there is nothing you can do about incorrect dunder methods, so they should be ignored.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Overridden incorrect dunder method was added to the tests to verify ruff doesn't catch it when evaluating the file. Snapshot changes are all just line number changes

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-09-07 16:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-09-12 11:44_

---

_Renamed from "Plw3201 ignore override" to "Ignore `@override` method when enforcing `bad-dunder-name` rule" by @charliermarsh on 2023-09-12 11:45_

---

_Label `bug` added by @charliermarsh on 2023-09-12 11:45_

---

_Comment by @charliermarsh on 2023-09-12 11:45_

Thank you!

---

_Comment by @codspeed-hq[bot] on 2023-09-12 11:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brendonh8:plw3201-ignore-override)

### Merging #7224 will **degrade performances by 5.38%**

<sub>Comparing <code>brendonh8:plw3201-ignore-override</code> (c00f140) with <code>main</code> (6661be2)</sub>



### Summary

`🔥 1` improvements
`❌ 5` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brendonh8:plw3201-ignore-override)._

### Benchmarks breakdown

|     | Benchmark | `main` | `brendonh8:plw3201-ignore-override` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🔥 | `lexer[large/dataset.py]` | 10.1 ms | 9.8 ms | +3.28% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 32.7 ms | 34.6 ms | -5.38% |
| ❌ | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.1 ms | -2.59% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 69.9 ms | 72.6 ms | -3.72% |
| ❌ | `linter/all-rules[large/dataset.py]` | 155.8 ms | 162.2 ms | -3.94% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 15.4 ms | 15.7 ms | -2.15% |


---

_Merged by @charliermarsh on 2023-09-12 11:54_

---

_Closed by @charliermarsh on 2023-09-12 11:54_

---
