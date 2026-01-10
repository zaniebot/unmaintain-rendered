```yaml
number: 11968
title: "[red-knot] Make the `VERSIONS` parser use `ModuleName` as its key type"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: versions-modulename
created_at: 2024-06-21T14:26:39Z
updated_at: 2024-06-21T15:56:51Z
url: https://github.com/astral-sh/ruff/pull/11968
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Make the `VERSIONS` parser use `ModuleName` as its key type

---

_Pull request opened by @AlexWaygood on 2024-06-21 14:26_

## Summary

The same invariants apply when constructing module names in the `VERSIONS` parser as the ones that apply when creating module names elsewhere in the module resolver. We should therefore reuse the same abstraction, which checks these invariants in its `new()` methods

## Test Plan

`cargo test -p red_knot_modules_resolver`


---

_Label `red-knot` added by @AlexWaygood on 2024-06-21 14:26_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-21 14:26_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-21 14:26_

---

_@MichaReiser approved on 2024-06-21 15:40_

---

_Merged by @AlexWaygood on 2024-06-21 15:46_

---

_Closed by @AlexWaygood on 2024-06-21 15:46_

---

_Branch deleted on 2024-06-21 15:46_

---

_Comment by @codspeed-hq[bot] on 2024-06-21 15:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/versions-modulename)

### Merging #11968 will **degrade performances by 4.82%**

<sub>Comparing <code>versions-modulename</code> (b730543) with <code>main</code> (8de0cd6)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/versions-modulename)._

### Benchmarks breakdown

|     | Benchmark | `main` | `versions-modulename` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.82% |


---

_Comment by @github-actions[bot] on 2024-06-21 15:56_

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
