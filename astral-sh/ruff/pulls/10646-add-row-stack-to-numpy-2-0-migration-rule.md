```yaml
number: 10646
title: "Add `row_stack` to NumPy 2.0 migration rule"
type: pull_request
state: merged
author: mtsokol
labels:
  - rule
assignees: []
merged: true
base: main
head: row_stack-numpy-rule
created_at: 2024-03-28T10:01:57Z
updated_at: 2024-03-28T13:56:38Z
url: https://github.com/astral-sh/ruff/pull/10646
synced_at: 2026-01-10T22:47:02Z
```

# Add `row_stack` to NumPy 2.0 migration rule

---

_Pull request opened by @mtsokol on 2024-03-28 10:01_

Hi! 

I left out one of the functions in the migration rule which became deprecated/removed in NumPy 2.0 (https://github.com/numpy/numpy/issues/26032#issuecomment-1999870797).

cc @anntzer

---

_Comment by @github-actions[bot] on 2024-03-28 10:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+11 -36 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+11 -36 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L119'>pandas/tests/frame/methods/test_quantile.py:119:9:</a> PLR6301 Method `test_axis_numeric_only_true` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L121'>pandas/tests/frame/methods/test_quantile.py:121:9:</a> PLR6301 Method `test_axis_numeric_only_true` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L133'>pandas/tests/frame/methods/test_quantile.py:133:9:</a> PLR6301 Method `test_quantile_date_range` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L135'>pandas/tests/frame/methods/test_quantile.py:135:9:</a> PLR6301 Method `test_quantile_date_range` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L149'>pandas/tests/frame/methods/test_quantile.py:149:9:</a> PLR6301 Method `test_quantile_axis_mixed` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L151'>pandas/tests/frame/methods/test_quantile.py:151:9:</a> PLR6301 Method `test_quantile_axis_mixed` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L173'>pandas/tests/frame/methods/test_quantile.py:173:9:</a> PLR6301 Method `test_quantile_axis_parameter` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L175'>pandas/tests/frame/methods/test_quantile.py:175:9:</a> PLR6301 Method `test_quantile_axis_parameter` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L211'>pandas/tests/frame/methods/test_quantile.py:211:9:</a> PLR6301 Method `test_quantile_interpolation` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L213'>pandas/tests/frame/methods/test_quantile.py:213:9:</a> PLR6301 Method `test_quantile_interpolation` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L221'>pandas/tests/frame/methods/test_quantile.py:221:9:</a> F821 Undefined name `ex`
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L22'>pandas/tests/frame/methods/test_quantile.py:22:1:</a> PLR0904 Too many public methods (29 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L270'>pandas/tests/frame/methods/test_quantile.py:270:9:</a> PLR6301 Method `test_quantile_interpolation_datetime` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L278'>pandas/tests/frame/methods/test_quantile.py:278:9:</a> PLR6301 Method `test_quantile_interpolation_int` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L291'>pandas/tests/frame/methods/test_quantile.py:291:9:</a> PLR6301 Method `test_quantile_multi` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L304'>pandas/tests/frame/methods/test_quantile.py:304:9:</a> PLR6301 Method `test_quantile_multi_axis_1` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L317'>pandas/tests/frame/methods/test_quantile.py:317:9:</a> PLR6301 Method `test_quantile_multi_empty` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L327'>pandas/tests/frame/methods/test_quantile.py:327:9:</a> PLR6301 Method `test_quantile_datetime` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/da802479fca521b7992fbf76d5984992cd5d495d/pandas/tests/frame/methods/test_quantile.py#L389'>pandas/tests/frame/methods/test_quantile.py:389:9:</a> PLR6301 Method `test_quantile_dt64_empty` could be a function, class method, or static method
... 29 additional changes omitted for rule PLR6301
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 45 | 10 | 35 | 0 | 0 |
| F821 | 1 | 1 | 0 | 0 | 0 |
| PLR0904 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @charliermarsh on 2024-03-28 13:49_

---

_Merged by @charliermarsh on 2024-03-28 13:49_

---

_Closed by @charliermarsh on 2024-03-28 13:49_

---

_Comment by @charliermarsh on 2024-03-28 13:49_

Thanks!

---

_Branch deleted on 2024-03-28 13:56_

---
