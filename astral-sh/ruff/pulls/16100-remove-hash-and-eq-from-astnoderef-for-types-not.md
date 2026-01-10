```yaml
number: 16100
title: "Remove `Hash` and `Eq` from `AstNodeRef` for types not implementing `Eq` or `Hash`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/revert-astnoderef-eq-hash-change
created_at: 2025-02-11T18:00:43Z
updated_at: 2025-02-11T18:55:53Z
url: https://github.com/astral-sh/ruff/pull/16100
synced_at: 2026-01-10T19:57:22Z
```

# Remove `Hash` and `Eq` from `AstNodeRef` for types not implementing `Eq` or `Hash`

---

_Pull request opened by @MichaReiser on 2025-02-11 18:00_

## Summary

This is a follow up to https://github.com/astral-sh/ruff/pull/15763#discussion_r1949681336

It reverts the change to using ptr equality for `AstNodeRef`s, which in turn removes the `Eq`, `PartialEq`, and `Hash` implementations for `AstNodeRef`s parametrized with AST nodes. 
Cheap comparisons shouldn't be needed because the node field is generally marked as `[#tracked]` and `#[no_eq]` and removing the implementations even enforces that those 
attributes are set on all `AstNodeRef` fields (which is good).

The only downside this has is that we technically wouldn't have to mark the `Unpack::target` as `#[tracked]` because 
the `target` field is accessed in every query accepting `Unpack` as an argument. 

Overall, enforcing the use of `#[tracked]` seems like a good trade off, espacially considering that it's very likely that
we'd probably forget to mark the `Unpack::target` field as tracked if we add a new `Unpack` query that doesn't access the target.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-02-11 18:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-11 18:00_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-11 18:00_

---

_Label `internal` added by @MichaReiser on 2025-02-11 18:00_

---

_Label `red-knot` added by @MichaReiser on 2025-02-11 18:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/ast_node_ref.rs`:93 on 2025-02-11 18:16_

What is the cost of this comparison? It doesn't seem like `ParsedModule` implements `Eq` -- does this comparison work because it implements `Deref` to `Parsed<ModModule>`? In that case it seems like it might be doing a deep compare of the entire AST? But I'm probably missing something.

---

_@carljm approved on 2025-02-11 18:17_

Looks good, modulo the one inline question.

To be clear, my comment wasn't intended to suggest that I think it's important we make this change; I was just curious what the tradeoffs are and if there was anything we should document for future.

---

_@MichaReiser reviewed on 2025-02-11 18:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/ast_node_ref.rs`:93 on 2025-02-11 18:24_

`ParsedModule` does a shallow compare

https://github.com/astral-sh/ruff/blob/69d86d1d69f3ef48f691b626595a7d0c056b9e66/crates/ruff_db/src/parsed.rs#L76-L80

---

_@carljm reviewed on 2025-02-11 18:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/ast_node_ref.rs`:93 on 2025-02-11 18:27_

Oh oops, I was looking at my local copy without the latest changes from the coarse-grained structs PR.

---

_Comment by @MichaReiser on 2025-02-11 18:29_

Hmm, it does seem that this regresses performance slightly (2%) for the cold case. Let me re-run to verify that it is real. It might just be the cost of tracking the `Unpack::target` field.

---

_Merged by @MichaReiser on 2025-02-11 18:55_

---

_Closed by @MichaReiser on 2025-02-11 18:55_

---

_Branch deleted on 2025-02-11 18:55_

---
