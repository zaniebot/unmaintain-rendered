```yaml
number: 11667
title: "[red-knot] infer_symbol_public_type infers union of all definitions"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
base: main
head: cfg
created_at: 2024-06-01T00:06:41Z
updated_at: 2024-06-01T01:57:35Z
url: https://github.com/astral-sh/ruff/pull/11667
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] infer_symbol_public_type infers union of all definitions

---

_Pull request opened by @carljm on 2024-06-01 00:06_

## Summary

Rename `infer_symbol_type` to `infer_symbol_public_type`, and allow it to work on symbols with more than one definition. For now, use the most cautious/sound inference, which is the union of all definitions. We can prune this union more in future by eliminating definitions if we can show that they can't be visible (this requires both that the symbol is definitely later reassigned, and that there is no intervening call/import that might be able to see the over-written definition).

## Test Plan

Added a test showing inference of union from multiple definitions.

---

_Label `red-knot` added by @carljm on 2024-06-01 00:06_

---

_Review requested from @MichaReiser by @carljm on 2024-06-01 00:06_

---

_Comment by @codspeed-hq[bot] on 2024-06-01 00:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/carljm:cfg)

### Merging #11667 will **improve performances by 11.01%**

<sub>Comparing <code>carljm:cfg</code> (18a64d8) with <code>main</code> (b80bf22)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `carljm:cfg` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1.2 ms | 1 ms | +11.01% |


---

_Comment by @github-actions[bot] on 2024-06-01 00:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @carljm on 2024-06-01 01:33_

---

_Comment by @carljm on 2024-06-01 01:57_

Closing in favor of https://github.com/astral-sh/ruff/pull/11669, which uses a source branch in the Astral repo instead of my own repo; this lets me stack PRs.

---

_Closed by @carljm on 2024-06-01 01:57_

---

_Branch deleted on 2024-06-01 01:57_

---
