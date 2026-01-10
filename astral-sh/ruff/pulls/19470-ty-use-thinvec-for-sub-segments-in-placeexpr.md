```yaml
number: 19470
title: "[ty] Use `ThinVec` for sub segments in `PlaceExpr`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/place-table-thin-vec
created_at: 2025-07-21T17:51:44Z
updated_at: 2025-07-22T18:49:11Z
url: https://github.com/astral-sh/ruff/pull/19470
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Use `ThinVec` for sub segments in `PlaceExpr`

---

_Pull request opened by @MichaReiser on 2025-07-21 17:51_

## Summary

This replaces the `SmallVec` on `PlaceExpr` with a `ThinVec`. The main motivation is that many places (all symbols) have no sub segments and paying the extra cost of an empty vec (32 bytes) for each of those adds quiet a bit to overall memory usage

This reduces the `place_table` size by roughly 25%

---

_Label `internal` added by @MichaReiser on 2025-07-21 17:51_

---

_Label `ty` added by @MichaReiser on 2025-07-21 17:51_

---

_Review requested from @carljm by @MichaReiser on 2025-07-21 17:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-21 17:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-21 17:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-21 17:51_

---

_Label `internal` added by @MichaReiser on 2025-07-21 17:51_

---

_Label `ty` added by @MichaReiser on 2025-07-21 17:51_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-07-21 17:51_

---

_Converted to draft by @MichaReiser on 2025-07-21 17:51_

---

_@MichaReiser reviewed on 2025-07-21 17:54_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:54 on 2025-07-21 17:54_

I already PRd (and it's merged but not yet released) adding `thin_vec` support to the getsize crate

---

_Comment by @github-actions[bot] on 2025-07-21 17:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo fields = ~236MB
+     memo fields = ~224MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~626MB
+ TOTAL MEMORY USAGE: ~596MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-07-21 18:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2025-07-21 18:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:789 on 2025-07-21 18:14_

This `SmallVec` here is "free" as 4 `ScopedPlaceId`s + the tag fit into 24 bytes

---

_@MichaReiser reviewed on 2025-07-21 18:14_

---

_Comment by @AlexWaygood on 2025-07-21 18:16_

Nice, looks like this yields some small speedups too! https://codspeed.io/astral-sh/ruff/branches/micha%2Fplace-table-thin-vec?runnerMode=Instrumentation

---

_@AlexWaygood approved on 2025-07-21 18:25_

---

_Comment by @AlexWaygood on 2025-07-21 18:30_

I think `PlaceExprSubSegment::StringSubscript` could also be relatively easily changed so that it wraps a `Box<str>` rather than a `String` -- not sure if that's worth doing or not

---

_Comment by @MichaReiser on 2025-07-21 19:30_

> I think PlaceExprSubSegment::StringSubscript could also be relatively easily changed so that it wraps a Box<str> rather than a String -- not sure if that's worth doing or not

This, unfortunately, doesn't help because `Name` is as big as a `String`.


An alternative to what I've done here is to split `PlaceTable` and internally track:

* `symbol_map`: `HashTable<ScopedSymbolId>`, 
* `symbols`: `IndexVec<ScopedSymbolId, Symbol>`
* `member_map`: `HashTable<ScopedMemberId>`,
* `members`: `IndexVec<ScopedMemberId, Member>`, Can use a `SmallVec` because each element has at least one item.
* `ScopedPlaceId` becomes an enum over `ScopedSymbolId` and `ScopedMemberId`. 

I suspect that this might be slightly larger refactor but it might also help to untangle some of the complexity in `PlaceTable`

---

_@carljm approved on 2025-07-21 22:16_

Nice!

---

_Comment by @carljm on 2025-07-21 22:17_

> An alternative to what I've done here is to split `PlaceTable`

Yes, I thought about this when reviewing the PR that added `PlaceTable`; I think it might be a nice refactor.

---

_Comment by @MichaReiser on 2025-07-22 05:51_

> Yes, I thought about this when reviewing the PR that added PlaceTable; I think it might be a nice refactor.

I'll try to find time to look into this because it might allow us to reduce memory usage even more by changing the `Member` definition from `Name`, `Segments` to `ScopedSymbolId`, `Segments` or just a `Segments` portion

---

_Comment by @MichaReiser on 2025-07-22 18:17_

> Yes, I thought about this when reviewing the PR that added PlaceTable; I think it might be a nice refactor.

I think this would be a very nice refactor but it's also not trivial. I started and I've currently 188 errors... and many of them aren't very trivial to fix.

I do think that it would help clarify many things and having separate types for `Symbol` and `Member` even allows stricter typing in many cases. But, as it often is with these kind of refactors, introducing those abstractions later is sort of a pain

---

_Merged by @MichaReiser on 2025-07-22 18:39_

---

_Closed by @MichaReiser on 2025-07-22 18:39_

---

_Branch deleted on 2025-07-22 18:39_

---

_Comment by @MichaReiser on 2025-07-22 18:49_

The main blocker here is that `UseDefMap` uses a bunch of `IndexVec<ScopedPlaceId>`. That prevents us from splitting the `places` `IndexVec` into two. 

We could split the hash map but there's little value in that because the main motivation is to reduce the size of `PlaceExpr` which is stored in the `IndexVec`.

---
