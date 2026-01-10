```yaml
number: 11975
title: "Revert \"[red-knot] Add more tests asserting that the VendoredFileSystem and the `VERSIONS` parser work with the vendored typeshed stubs\""
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: revert-11970-typeshed-validation
created_at: 2024-06-21T19:10:23Z
updated_at: 2024-06-21T19:30:11Z
url: https://github.com/astral-sh/ruff/pull/11975
synced_at: 2026-01-10T21:56:00Z
```

# Revert "[red-knot] Add more tests asserting that the VendoredFileSystem and the `VERSIONS` parser work with the vendored typeshed stubs"

---

_Pull request opened by @AlexWaygood on 2024-06-21 19:10_

Reverts astral-sh/ruff#11970

The tests failed on Windows. I'll have another go tomorrow; for now this is a revert to get things green on `main` again.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-21 19:10_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-21 19:10_

---

_@carljm approved on 2024-06-21 19:11_

---

_Comment by @AlexWaygood on 2024-06-21 19:12_

Here's a PR to my fork with some debug output that shows what's going wrong, FWIW: we have some cursed combination of `\` and `/` separators in the vendored zip file on Windows. https://github.com/AlexWaygood/ruff/pull/10

---

_Merged by @AlexWaygood on 2024-06-21 19:14_

---

_Closed by @AlexWaygood on 2024-06-21 19:14_

---

_Branch deleted on 2024-06-21 19:14_

---

_Comment by @codspeed-hq[bot] on 2024-06-21 19:14_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/revert-11970-typeshed-validation)

### Merging #11975 will **degrade performances by 4.82%**

<sub>Comparing <code>revert-11970-typeshed-validation</code> (44f34f0) with <code>main</code> (791f6a1)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/revert-11970-typeshed-validation)._

### Benchmarks breakdown

|     | Benchmark | `main` | `revert-11970-typeshed-validation` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.82% |


---

_Comment by @github-actions[bot] on 2024-06-21 19:30_

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
