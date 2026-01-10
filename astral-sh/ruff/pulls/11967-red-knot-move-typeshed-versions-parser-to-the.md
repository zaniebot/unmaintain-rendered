```yaml
number: 11967
title: "[red-knot] Move typeshed `VERSIONS` parser to the module resolver crate"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: move-versions-logic
created_at: 2024-06-21T14:15:50Z
updated_at: 2024-06-21T15:41:09Z
url: https://github.com/astral-sh/ruff/pull/11967
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Move typeshed `VERSIONS` parser to the module resolver crate

---

_Pull request opened by @AlexWaygood on 2024-06-21 14:15_

## Summary

This PR moves the `VERSIONS` parser from the `red_knot` crate to the `red_knot_module_resolver` crate, so that it can be used in the module resolver.

---

_Label `red-knot` added by @AlexWaygood on 2024-06-21 14:15_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-21 14:15_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-21 14:15_

---

_Comment by @github-actions[bot] on 2024-06-21 14:35_

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

_@MichaReiser approved on 2024-06-21 15:40_

---

_Merged by @AlexWaygood on 2024-06-21 15:41_

---

_Closed by @AlexWaygood on 2024-06-21 15:41_

---

_Branch deleted on 2024-06-21 15:41_

---
