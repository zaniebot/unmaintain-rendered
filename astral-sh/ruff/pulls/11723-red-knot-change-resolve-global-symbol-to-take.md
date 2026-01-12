```yaml
number: 11723
title: "red-knot: Change `resolve_global_symbol` to take `Module` as an argument"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: resolve-global-symbol-accept-module
created_at: 2024-06-03T15:20:03Z
updated_at: 2024-06-04T06:32:15Z
url: https://github.com/astral-sh/ruff/pull/11723
synced_at: 2026-01-12T15:55:38Z
```

# red-knot: Change `resolve_global_symbol` to take `Module` as an argument

---

_@MichaReiser_

## Summary

This PR changes the signature of `resolve_global_symbol` to accept a `Module` instead of a `ModuleName` to avoid that we go through `module.name(db)` -> `resolve_module(db, name)` when we areadly have a module. 

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-06-03 15:20_

---

_Label `red-knot` added by @MichaReiser on 2024-06-03 15:23_

---

_Comment by @github-actions[bot] on 2024-06-03 15:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-06-03 19:20_

Makes sense, looks good!

---

_Merged by @MichaReiser on 2024-06-04 06:20_

---

_Closed by @MichaReiser on 2024-06-04 06:20_

---

_Branch deleted on 2024-06-04 06:20_

---

_Comment by @codspeed-hq[bot] on 2024-06-04 06:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/resolve-global-symbol-accept-module)

### Merging #11723 will **degrade performances by 12.14%**

<sub>Comparing <code>resolve-global-symbol-accept-module</code> (b1fcabc) with <code>main</code> (64165be)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/resolve-global-symbol-accept-module)._

### Benchmarks breakdown

|     | Benchmark | `main` | `resolve-global-symbol-accept-module` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 801.2 µs | 911.9 µs | -12.14% |


---
