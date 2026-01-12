```yaml
number: 11957
title: Move token and error structs into related modules
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/lexer-move
created_at: 2024-06-21T05:15:28Z
updated_at: 2024-06-21T10:23:12Z
url: https://github.com/astral-sh/ruff/pull/11957
synced_at: 2026-01-12T15:55:39Z
```

# Move token and error structs into related modules

---

_@dhruvmanila_

## Summary

This PR does some housekeeping into moving certain structs into related modules. Specifically,
1. Move `LexicalError` from `lexer.rs` to `error.rs` which also contains the `ParseError`
2. Move `Token`, `TokenFlags` and `TokenValue` from `lexer.rs` to `token.rs`


---

_Label `internal` added by @dhruvmanila on 2024-06-21 05:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-21 05:15_

---

_Comment by @github-actions[bot] on 2024-06-21 05:45_

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

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2024-06-21 06:00_

---

_Merged by @dhruvmanila on 2024-06-21 10:07_

---

_Closed by @dhruvmanila on 2024-06-21 10:07_

---

_Branch deleted on 2024-06-21 10:07_

---
