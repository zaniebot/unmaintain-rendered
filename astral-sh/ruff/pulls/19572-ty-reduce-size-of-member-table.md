```yaml
number: 19572
title: "[ty] Reduce size of member table"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/member-size
created_at: 2025-07-26T18:29:16Z
updated_at: 2025-08-07T09:21:22Z
url: https://github.com/astral-sh/ruff/pull/19572
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Reduce size of member table

---

_@MichaReiser_

## Summary

This PR shrinks the size of the members table by ~28% (from 611MB to 438MB on a large project). 

This is achieved by changing how we store `MemberExpr`. 

**Before**

```rust
pub(crate) struct MemberExpr {
    symbol_name: Name,
	segments: SmallVec<[MemberSegment; 1]>,
}

pub(crate) enum MemberSegment {
    /// An attribute access, e.g. `.y` in `x.y`
    Attribute(Name),
    /// An integer-based index access, e.g. `[1]` in `x[1]`
    IntSubscript(ast::Int),
    /// A string-based index access, e.g. `["foo"]` in `x["foo"]`
    StringSubscript(String),
}
```

There are a few issues with this layout: `MemberSegment` itself is 32 bytes because it stores a `String` (or int) for each segment. This is an issue for multiple reasons:

1. We can only store a single segment inline
2. Even then, the small vec with only a single inline segment accounts for 40 bytes (32 bytes for a single segment + its metadata).
3. Each heap allocated segment also takes 32 bytes


This PR shrinks the size of `MemberExpr` and `MemberSegment` and reduces the cases where heap allocating the segments is necessary.


**New**

```rust
pub(crate) struct MemberExpr {
    /// The entire path as a single Name
    path: Name,
    /// Metadata for each segment (in forward order)
    segments: Segments,
}
```

The first change is that we change `symbol_name` to a path. The path stores the entire path of the member expression. For example, `a.b[4]` becomes `ab4`. This avoids the need for each segment to store its own `Name` (or `Int`)


The second important change is that `Segments` is now an enum over a small and a heap variant:

```rust
#[derive(Clone, Debug, PartialEq, Eq, get_size2::GetSize)]
enum Segments {
    /// Inline storage for up to 7 segments with 6-bit relative offsets (max 63 bytes per segment)
    Small(SmallSegments),
    /// Heap storage for expressions that don't fit inline
    Heap(Box<[SegmentInfo]>),
}
```

`SmallSegments` packs up to 7 segments into a single `u64` (see docstring for more details). 

`SegmentInfo` packs both an offset and a kind, so that a single segment only takes 4 bytes (it's a `u32`).


All these improvements together shrink `MemberExpr` from 64 bytes to 40 bytes




---

_Label `ty` added by @MichaReiser on 2025-07-26 18:30_

---

_Comment by @github-actions[bot] on 2025-07-26 18:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~167MB
+ TOTAL MEMORY USAGE: ~159MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo fields = ~214MB
+     memo fields = ~204MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-07-26 18:41_

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

_Comment by @github-actions[bot] on 2025-08-03 08:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Renamed from "[ty] Prototype shrink MemberSize" to "[ty] Reduce size of memory table" by @MichaReiser on 2025-08-03 14:33_

---

_Marked ready for review by @MichaReiser on 2025-08-06 09:46_

---

_Review requested from @carljm by @MichaReiser on 2025-08-06 09:46_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-06 09:46_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-06 09:46_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-06 09:46_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-06 10:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/member.rs`:705 on 2025-08-06 17:16_

This comment doesn't quite make sense to me here? At this point we've already determined that we can fit in inline storage; the checks are above.

---

_@carljm approved on 2025-08-06 17:17_

Nice!

---

_Renamed from "[ty] Reduce size of memory table" to "[ty] Reduce size of member table" by @MichaReiser on 2025-08-06 18:07_

---

_@MichaReiser reviewed on 2025-08-07 09:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/member.rs`:705 on 2025-08-07 09:08_

That's what you get for writing large parts of the implementation with claude :) Thanks for catching this

---

_Merged by @MichaReiser on 2025-08-07 09:16_

---

_Closed by @MichaReiser on 2025-08-07 09:16_

---

_Branch deleted on 2025-08-07 09:16_

---
