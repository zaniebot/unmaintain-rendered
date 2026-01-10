```yaml
number: 12924
title: "[`flake8-async`] Fix examples to use `async with`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-async-doc
created_at: 2024-08-16T10:12:42Z
updated_at: 2024-08-16T10:26:35Z
url: https://github.com/astral-sh/ruff/pull/12924
synced_at: 2026-01-10T21:38:32Z
```

# [`flake8-async`] Fix examples to use `async with`

---

_Pull request opened by @MichaReiser on 2024-08-16 10:12_

## Summary
Fixes https://github.com/astral-sh/ruff/issues/12921



---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-16 10:13_

---

_Label `documentation` added by @MichaReiser on 2024-08-16 10:13_

---

_@AlexWaygood approved on 2024-08-16 10:22_

---

_Merged by @MichaReiser on 2024-08-16 10:24_

---

_Closed by @MichaReiser on 2024-08-16 10:24_

---

_Branch deleted on 2024-08-16 10:25_

---

_Comment by @github-actions[bot] on 2024-08-16 10:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -33 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -33 violations, +0 -0 fixes)</summary>
<p>

<pre>
- doc/source/user_guide/style.ipynb:cell 113:31:89: E501 Line too long (90 > 88)
- doc/source/user_guide/style.ipynb:cell 113:33:89: E501 Line too long (96 > 88)
- doc/source/user_guide/style.ipynb:cell 123:3:56: E741 Ambiguous variable name: `l`
- doc/source/user_guide/style.ipynb:cell 125:2:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:4:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:6:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:8:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 126:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 126:2:1: PLW0127 Self-assignment of variable `cmap`
- doc/source/user_guide/style.ipynb:cell 126:2:8: PLW0128 Redeclared variable `cmap` in assignment
- doc/source/user_guide/style.ipynb:cell 126:3:22: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 128:1:22: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 134:1:89: E501 Line too long (93 > 88)
- doc/source/user_guide/style.ipynb:cell 141:1:89: E501 Line too long (92 > 88)
- doc/source/user_guide/style.ipynb:cell 157:1:38: F811 Redefinition of unused `f` from cell 123, line 3
- doc/source/user_guide/style.ipynb:cell 15:2:89: E501 Line too long (103 > 88)
- doc/source/user_guide/style.ipynb:cell 15:3:89: E501 Line too long (155 > 88)
- doc/source/user_guide/style.ipynb:cell 16:48:89: E501 Line too long (103 > 88)
- doc/source/user_guide/style.ipynb:cell 16:52:89: E501 Line too long (162 > 88)
... 10 additional changes omitted for rule E501
- doc/source/user_guide/style.ipynb:cell 37:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 37:2:20: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 60:1:20: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 6:1:27: NPY002 Replace legacy `np.random.rand` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 9:1:19: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
... 1 additional changes omitted for rule NPY002
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E501 | 17 | 0 | 17 | 0 | 0 |
| NPY002 | 8 | 0 | 8 | 0 | 0 |
| C408 | 4 | 0 | 4 | 0 | 0 |
| E741 | 1 | 0 | 1 | 0 | 0 |
| PLW0127 | 1 | 0 | 1 | 0 | 0 |
| PLW0128 | 1 | 0 | 1 | 0 | 0 |
| F811 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
