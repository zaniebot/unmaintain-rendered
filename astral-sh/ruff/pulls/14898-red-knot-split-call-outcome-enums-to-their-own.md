```yaml
number: 14898
title: "[red-knot] split call-outcome enums to their own submodule"
type: pull_request
state: merged
author: carljm
labels: []
assignees: []
merged: true
base: main
head: cjm/splitcall
created_at: 2024-12-10T19:49:14Z
updated_at: 2024-12-10T20:03:31Z
url: https://github.com/astral-sh/ruff/pull/14898
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] split call-outcome enums to their own submodule

---

_Pull request opened by @carljm on 2024-12-10 19:49_

## Summary

This is already several hundred lines of code, and it will get more complex with call-signature checking.

## Test Plan

This is a pure code move; the moved code wasn't changed, just imports. Existing tests pass.

---

_Review requested from @MichaReiser by @carljm on 2024-12-10 19:49_

---

_Review requested from @AlexWaygood by @carljm on 2024-12-10 19:49_

---

_Review requested from @sharkdp by @carljm on 2024-12-10 19:49_

---

_Comment by @github-actions[bot] on 2024-12-10 19:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/ca91dd4c39a02c0026b98c16c56996f81506e004/pandas/io/formats/style.py#L1147'>pandas/io/formats/style.py:1147:13:</a> RUF052 Local dummy variable `_original_columns` is accessed
- <a href='https://github.com/pandas-dev/pandas/blob/ca91dd4c39a02c0026b98c16c56996f81506e004/pandas/io/formats/style.py#L145'>pandas/io/formats/style.py:145:1:</a> E113 Unexpected indentation
- <a href='https://github.com/pandas-dev/pandas/blob/ca91dd4c39a02c0026b98c16c56996f81506e004/pandas/io/formats/style.py#L145'>pandas/io/formats/style.py:145:45:</a> W292 No newline at end of file
+ <a href='https://github.com/pandas-dev/pandas/blob/ca91dd4c39a02c0026b98c16c56996f81506e004/pandas/io/formats/style.py#L3928'>pandas/io/formats/style.py:3928:5:</a> RUF052 Local dummy variable `_matplotlib` is accessed
+ <a href='https://github.com/pandas-dev/pandas/blob/ca91dd4c39a02c0026b98c16c56996f81506e004/pandas/io/formats/style.py#L4112'>pandas/io/formats/style.py:4112:89:</a> E501 Line too long (91 > 88)
+ <a href='https://github.com/pandas-dev/pandas/blob/ca91dd4c39a02c0026b98c16c56996f81506e004/pandas/io/formats/style.py#L4217'>pandas/io/formats/style.py:4217:9:</a> RUF052 Local dummy variable `_matplotlib` is accessed
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF052 | 3 | 3 | 0 | 0 | 0 |
| E501 | 1 | 1 | 0 | 0 | 0 |
| E113 | 1 | 0 | 1 | 0 | 0 |
| W292 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @carljm on 2024-12-10 20:03_

---

_Closed by @carljm on 2024-12-10 20:03_

---

_Branch deleted on 2024-12-10 20:03_

---
