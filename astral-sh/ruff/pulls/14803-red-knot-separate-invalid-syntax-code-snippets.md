```yaml
number: 14803
title: "[red-knot] Separate invalid syntax code snippets"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/separate-code-snippets
created_at: 2024-12-06T02:37:04Z
updated_at: 2024-12-06T02:42:52Z
url: https://github.com/astral-sh/ruff/pull/14803
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Separate invalid syntax code snippets

---

_@dhruvmanila_

Ref: https://github.com/astral-sh/ruff/pull/14788#discussion_r1872242283

This PR:
* Separates code snippets as individual tests for the invalid syntax cases
* Adds a general comment explaining why the parser could emit more syntax errors than expected

---

_Label `red-knot` added by @dhruvmanila on 2024-12-06 02:37_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-06 02:37_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-06 02:37_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-06 02:37_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-06 02:37_

---

_Merged by @dhruvmanila on 2024-12-06 02:41_

---

_Closed by @dhruvmanila on 2024-12-06 02:41_

---

_Branch deleted on 2024-12-06 02:41_

---

_Comment by @github-actions[bot] on 2024-12-06 02:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -8 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L1696'>pandas/core/algorithms.py:1696:1:</a> E305 Expected 2 blank lines after class or function definition, found (0)
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L27'>pandas/core/algorithms.py:27:5:</a> TC001 Move application import `pandas._typing.AnyArrayLike` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L28'>pandas/core/algorithms.py:28:5:</a> TC001 Move application import `pandas._typing.ArrayLike` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L29'>pandas/core/algorithms.py:29:5:</a> TC001 Move application import `pandas._typing.ArrayLikeT` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L30'>pandas/core/algorithms.py:30:5:</a> TC001 Move application import `pandas._typing.AxisInt` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L31'>pandas/core/algorithms.py:31:5:</a> TC001 Move application import `pandas._typing.DtypeObj` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L32'>pandas/core/algorithms.py:32:5:</a> TC001 Move application import `pandas._typing.TakeIndexer` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L33'>pandas/core/algorithms.py:33:5:</a> TC001 Move application import `pandas._typing.npt` into a type-checking block
- <a href='https://github.com/pandas-dev/pandas/blob/a36c44e129bd2f70c25d5dec89cb2893716bdbf6/pandas/core/algorithms.py#L916'>pandas/core/algorithms.py:916:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC001 | 7 | 0 | 7 | 0 | 0 |
| E305 | 1 | 1 | 0 | 0 | 0 |
| PLR6104 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---
