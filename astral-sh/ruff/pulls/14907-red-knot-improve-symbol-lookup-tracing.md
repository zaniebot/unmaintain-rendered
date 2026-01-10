```yaml
number: 14907
title: "[red-knot] Improve symbol-lookup tracing"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/tracing-symbol-lookups
created_at: 2024-12-11T08:41:18Z
updated_at: 2025-01-07T13:54:23Z
url: https://github.com/astral-sh/ruff/pull/14907
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Improve symbol-lookup tracing

---

_Pull request opened by @sharkdp on 2024-12-11 08:41_

## Summary

When debugging, I frequently want to know which symbols are being looked up. `symbol_by_id` adds tracing information, but it only shows the `ScopedSymbolId`. Since `symbol_by_id` is only called from `symbol`, it seems reasonable to move the tracing call one level up from `symbol_by_id` to `symbol`, where we can also show the name of the symbol.

**Before**:

```
6      └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(60de), file=/home/shark/tomllib_modified/_parser.py}
6        └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(33)}
6        ┌─┘
6        └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(123)}
6        ┌─┘
6        └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(54)}
6        ┌─┘
6        └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(122)}
6        ┌─┘
6        └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(165)}
6        ┌─┘
6      ┌─┘
6      └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(32)}
6      ┌─┘
6      └─┐red_knot_python_semantic::types::symbol_by_id{symbol=ScopedSymbolId(232)}
6      ┌─┘
6    ┌─┘
6  ┌─┘
6┌─┘
```

**After**:

```
5      └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(60de), file=/home/shark/tomllib_modified/_parser.py}
5        └─┐red_knot_python_semantic::types::symbol{name="dict"}
5        ┌─┘
5        └─┐red_knot_python_semantic::types::symbol{name="dict"}
5        ┌─┘
5        └─┐red_knot_python_semantic::types::symbol{name="list"}
5        ┌─┘
5        └─┐red_knot_python_semantic::types::symbol{name="list"}
5        ┌─┘
5        └─┐red_knot_python_semantic::types::symbol{name="isinstance"}
5        ┌─┘
5        └─┐red_knot_python_semantic::types::symbol{name="isinstance"}
5        ┌─┘
5      ┌─┘
5      └─┐red_knot_python_semantic::types::symbol{name="ValueError"}
5      ┌─┘
5      └─┐red_knot_python_semantic::types::symbol{name="ValueError"}
5      ┌─┘
5    ┌─┘
5  ┌─┘
5┌─┘
```

## Test Plan

```
cargo run --bin red_knot -- --current-directory path/to/tomllib -vvv
```

---

_Label `red-knot` added by @sharkdp on 2024-12-11 08:41_

---

_Review requested from @carljm by @sharkdp on 2024-12-11 08:41_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-11 08:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-11 08:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:79 on 2024-12-11 08:44_

I'm inclined to inline the entire `symbol_by_id` function into `symbol` to avoid "missing" symbol lookups in tracing because a symbol gets looked up by its id.

---

_@MichaReiser approved on 2024-12-11 08:44_

---

_Comment by @github-actions[bot] on 2024-12-11 08:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-12-11 11:00_

I agree with Micha's comment, but LGTM otherwise. (Wow, that output is so much more useful!)

---

_@carljm approved on 2024-12-11 16:25_

So much better, thank you!

---

_@sharkdp reviewed on 2025-01-07 09:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:79 on 2025-01-07 09:33_

`symbol_by_id` is now `#[salsa::tracked]`, so I can't inline it trivially. Also, it feels a bit wrong to inline that nicely encapsulated into this lambda here:

https://github.com/astral-sh/ruff/blob/897c307f40f9909ac2b5c5db1194cbeda4d1750f/crates/red_knot_python_semantic/src/types.rs#L171-L175

I now pushed a version where I nested `symbol_by_id` inside `symbol`. I'm not a huge fan, but it solves your concern as well. What do you think @MichaReiser?

(The diff is horrible. The only thing I did was to move `symbol_by_id` inside `symbol` and adapted the doc comments. No code has been changed except for the removed/added `tracing::trace_span` line.

---

_@sharkdp reviewed on 2025-01-07 09:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:81 on 2025-01-07 09:35_

I tried to remove this (and use `scope` from the outer scope), but it seems to be required in order to make this salsa_tracked.

---

_@MichaReiser reviewed on 2025-01-07 09:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:81 on 2025-01-07 09:36_

This works for me. Thanks

---

_Merged by @sharkdp on 2025-01-07 09:41_

---

_Closed by @sharkdp on 2025-01-07 09:41_

---

_Branch deleted on 2025-01-07 09:41_

---

_@dcreager reviewed on 2025-01-07 13:45_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:79 on 2025-01-07 13:45_

> (The diff is horrible. The only thing I did was to move `symbol_by_id` inside `symbol` and adapted the doc comments. No code has been changed except for the removed/added `tracing::trace_span` line.

fyi if you click the gear icon at the top of the diff view, you can check "Hide whitespace", which makes this particular diff much cleaner

---

_@sharkdp reviewed on 2025-01-07 13:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:79 on 2025-01-07 13:54_

Thanks! I know about hide-whitespace, but somehow didn't think of enabling it for this changeset :+1: 

---
