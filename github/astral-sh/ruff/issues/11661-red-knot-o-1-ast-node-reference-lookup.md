---
number: 11661
title: "[red-knot] O(1) AST node reference lookup"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-05-31T22:10:55Z
updated_at: 2024-07-17T06:55:18Z
url: https://github.com/astral-sh/ruff/issues/11661
synced_at: 2026-01-07T13:12:15-06:00
---

# [red-knot] O(1) AST node reference lookup

---

_Issue opened by @carljm on 2024-05-31 22:10_

Definitions need to reference AST nodes, but actual references would require dealing with lifetimes, which is awkward given our database structure. Arc would be another option, but our AST doesn't use it internally, so that doesn't work either.

Currently we use `NodeKey`, which is just a node kind and text range, and requires binary search (at best) to find the node again. This is too slow.

We could use a struct that wraps an Arc to the root node of the AST and a raw pointer to the exact node. Since the whole AST is transitively owned by the root node, and ASTs are immutable, this can allow us to safely de-reference the raw pointer.

Or another solution might turn up as part of the salsa transition (e.g. we might decide to transform the AST into our own representation, which could use Arc internally?)

---

_Label `red-knot` added by @carljm on 2024-05-31 22:10_

---

_Assigned to @carljm by @carljm on 2024-05-31 22:10_

---

_Comment by @MichaReiser on 2024-07-17 06:55_

I did this as part of the salsa refactor

---

_Closed by @MichaReiser on 2024-07-17 06:55_

---
