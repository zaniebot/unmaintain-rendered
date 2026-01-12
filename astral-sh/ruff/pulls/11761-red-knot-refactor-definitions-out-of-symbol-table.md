```yaml
number: 11761
title: "[red-knot] refactor Definitions out of symbol table"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/defmod
created_at: 2024-06-05T20:19:36Z
updated_at: 2024-06-05T20:35:02Z
url: https://github.com/astral-sh/ruff/pull/11761
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] refactor Definitions out of symbol table

---

_@carljm_

## Summary

Definitions are used in symbol table and in flow graph, and aren't inherently owned by one or the other; move them into their own submodule.

## Test Plan

Existing tests.

---

_Review requested from @MichaReiser by @carljm on 2024-06-05 20:19_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-05 20:19_

---

_Label `red-knot` added by @carljm on 2024-06-05 20:19_

---

_@AlexWaygood approved on 2024-06-05 20:21_

---

_Comment by @codspeed-hq[bot] on 2024-06-05 20:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/defmod)

### Merging #11761 will **improve performances by 4.12%**

<sub>Comparing <code>cjm/defmod</code> (b2ba76a) with <code>main</code> (b46e9e8)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/defmod` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 835.8 µs | 802.7 µs | +4.12% |


---

_Comment by @github-actions[bot] on 2024-06-05 20:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-06-05 20:35_

---

_Closed by @carljm on 2024-06-05 20:35_

---

_Branch deleted on 2024-06-05 20:35_

---
