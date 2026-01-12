```yaml
number: 11973
title: "Remove usage of `std::path::absolute` from snapshot test"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/test/remove-path-absolute
created_at: 2024-06-21T17:42:41Z
updated_at: 2024-06-21T19:21:13Z
url: https://github.com/astral-sh/ruff/pull/11973
synced_at: 2026-01-12T15:55:40Z
```

# Remove usage of `std::path::absolute` from snapshot test

---

_@snowsignal_

## Summary

Using `std::path::absolute` in the Jupyter Notebook snapshot tests broke our MSRV checks: https://github.com/astral-sh/ruff/actions/runs/9608928482/job/26502534725?pr=11959.

This PR replaces the usage of `std::path::absolute` with `std::fs::canonicalize`, which should work identically in this case.

---

_Label `server` added by @snowsignal on 2024-06-21 17:42_

---

_Comment by @github-actions[bot] on 2024-06-21 17:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-06-21 19:14_

(I'm fixing the other CI failure in #11975!)

---

_@AlexWaygood approved on 2024-06-21 19:14_

---

_Merged by @MichaReiser on 2024-06-21 19:21_

---

_Closed by @MichaReiser on 2024-06-21 19:21_

---

_Branch deleted on 2024-06-21 19:21_

---
