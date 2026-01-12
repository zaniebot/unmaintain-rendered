```yaml
number: 16286
title: "[red-knot] Consolidate `SymbolBindings`/`SymbolDeclarations` state"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/fold-arrays
created_at: 2025-02-20T19:32:19Z
updated_at: 2025-02-20T21:20:26Z
url: https://github.com/astral-sh/ruff/pull/16286
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Consolidate `SymbolBindings`/`SymbolDeclarations` state

---

_@dcreager_

This updates the `SymbolBindings` and `SymbolDeclarations` types to use a single smallvec of live bindings/declarations, instead of splitting that out into separate containers for each field.

I'm seeing an 11-13% `cargo bench` performance improvement with this locally (for both cold and incremental).  I'm interested to see if Codspeed agrees!

---

_Label `internal` added by @dcreager on 2025-02-20 19:32_

---

_Label `red-knot` added by @dcreager on 2025-02-20 19:32_

---

_Review requested from @carljm by @dcreager on 2025-02-20 19:32_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-20 19:32_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-20 19:32_

---

_Review requested from @sharkdp by @dcreager on 2025-02-20 19:32_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 19:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ffold-arrays)

### Merging #16286 will **improve performances by 7.37%**

<sub>Comparing <code>dcreager/fold-arrays</code> (3859aeb) with <code>main</code> (470f852)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[cold] `` | 90.8 ms | 84.5 ms | +7.37% |


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:98 on 2025-02-20 19:48_

If we no longer need this method, let's just remove it and its tests? It'll always be there in git history if we need it again.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:29 on 2025-02-20 19:49_

Same as for `union`, maybe just remove this method if we no longer need it?

If it's a useful convenience for tests, maybe move it into a separate `impl` in the test module?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:451 on 2025-02-20 19:56_

The name `ConstraintIdIterator` (instead of `ConstraintIterator`) was meant to distinguish an iterator over internal constraint IDs from an iterator over `Constraint` Salsa ingredients (which of course are also just IDs 😆 but global ones). After this name change we now have `ConstraintsIterator` and `ConstraintIterator`, which seems... less clear. I guess you made this name change because it's now just an iterator over `u32` instead of `ScopedConstraintId`? Maybe `ConstraintIndexIterator`? Or maybe this doesn't matter...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:100 on 2025-02-20 20:00_

Nit: I think we usually try to not put other types in between a struct and its impl? But I don't feel strongly about this, if you feel this is the clearest ordering in this case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:140 on 2025-02-20 20:02_

```suggestion
    /// Iterate over the IDs of each currently live declaration for this symbol
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:261 on 2025-02-20 20:06_

```suggestion
    /// Iterate over the IDs of each currently live binding for this symbol
```

---

_@carljm approved on 2025-02-20 20:08_

This is awesome, thank you!!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:29 on 2025-02-20 20:51_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:98 on 2025-02-20 20:51_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:100 on 2025-02-20 20:52_

Yeah my thought was that these three types only make sense together, and so it's useful to see the type definitions next to each other.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:451 on 2025-02-20 20:54_

It wasn't as intentional as you're giving me credit for... :smile: 

This iterator comes from `symbol_state.rs` now, instead of being defined here, and I was trying to follow the naming convention of the other iterators there.  Although I now see that I didn't do that successfully — it should be `ConstraintsIterator`, since it seems like the pattern there is to name the iterator after the type being iterated, not the element type.  But that then conflicts with the iterator defined here...

Anyway I went with `ConstraintIndexIterator` as you suggest.

---

_@dcreager reviewed on 2025-02-20 20:59_

---

_Merged by @dcreager on 2025-02-20 21:20_

---

_Closed by @dcreager on 2025-02-20 21:20_

---

_Branch deleted on 2025-02-20 21:20_

---
