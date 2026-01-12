```yaml
number: 12900
title: "[red-knot] [WIP] SSA-style use-def map (unoptimized)"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
draft: true
base: main
head: cjm/phis
created_at: 2024-08-15T02:12:41Z
updated_at: 2024-10-18T18:12:32Z
url: https://github.com/astral-sh/ruff/pull/12900
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] [WIP] SSA-style use-def map (unoptimized)

---

_@carljm_

This uses the algorithm in https://c9x.me/compile/bib/braun13cc.pdf to do our use-def map in an SSA style, where there's always only one visible definition for a symbol, and we use Phi definitions at control-flow merge points to merge the visible definitions from each path.

In a sense this is a synthesis between the first version (where we built a CFG and traversed it lazily in type inference) and the current code. This still associates uses with definitions directly in semantic indexing rather than lazily traversing in type inference, but it does the use-def association using memoized lazy traversal of a CFG. 

Significant advantages I see of this approach:
- In scopes where we don’t need inferred public types for all symbols, it can save tracking metadata for symbols after the point where they are no longer used. 
- The Phi definitions allow us to cache union and intersection building and simplification (which will get more expensive as we add more to the type system and do proper subtype based simplification); in the current system this work is redone for every use of a symbol with multiple definitions/constraints. 
- It doesn’t require tracking an MxN matrix of definitions bs constraints for all symbols in building the use def map. 

It's a significant regression right now, but it's also missing some needed optimizations to avoid creating lots of unnecessary trivial Phi nodes.

---

_Review requested from @MichaReiser by @carljm on 2024-08-15 02:12_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-15 02:12_

---

_Comment by @codspeed-hq[bot] on 2024-08-15 02:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/phis)

### Merging #12900 will **degrade performances by 61.43%**

<sub>Comparing <code>cjm/phis</code> (bccc33c) with <code>main</code> (ac7b177)</sub>



### Summary

`❌ 2` regressions
`✅ 30` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cjm/phis)._

### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/phis` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[cold]` | 39 ms | 60.3 ms | -35.39% |
| ❌ | `red_knot_check_file[incremental]` | 1.6 ms | 4.1 ms | -61.43% |


---

_Converted to draft by @carljm on 2024-08-15 02:24_

---

_Comment by @github-actions[bot] on 2024-08-15 02:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `red-knot` added by @carljm on 2024-08-15 02:51_

---

_Renamed from "[WIP] working version of SSA-style use-def map" to "[red-knot] [WIP] SSA-style use-def map (unoptimized)" by @carljm on 2024-08-15 03:25_

---

_Comment by @MichaReiser on 2024-10-18 06:48_

@carljm should we close this PR as we don't plan on landing it as is. It wouldn't be gone. We can still find it in the closed PRs

---

_Closed by @carljm on 2024-10-18 18:12_

---
