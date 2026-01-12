```yaml
number: 14054
title: "[red-knot] Remove `Definition` from `Class` and `FunctionType`"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2024-11-02T17:18:43Z
updated_at: 2024-11-05T08:02:39Z
url: https://github.com/astral-sh/ruff/issues/14054
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] Remove `Definition` from `Class` and `FunctionType`

---

_@MichaReiser_

The `ClassType` and `FunctionType` both store a back reference to their origin definition, and the definition holds on to the AST node. This is problematic because `Type`s are visible cross-module:

* If a `Type` changes (no longer compares equal), it requires redoing type inference for all dependent types (modules). This gets worse because `Type`s flow and compose. E.g., every union containing a `ClassType` would have to be thrown away if the node's AST location changes (and, in turn, all results that depend on that Union...). Fortunately, this currently works thanks to Salsa tracking each struct field on its own. However, it will break if we change to use coarse-grained salsa dependencies for better performance.
* Incrementally checking a program after restoring from a persistent cache shouldn't require reparsing any unchanged modules. Instead, Red Knot resolves the types directly from the cache. For this to work efficiently, it's important that `Type`s have no backreference to the AST. Otherwise, deserializing the `Type` requires reparsing the module or storing and deserializing the AST in/from the persistent cache. But it's not just the cost of deserialization; this also comes at a cost that Red Knot now holds on to ASTs that are never needed. It shouldn't be necessary to revisit the AST of standard library modules after they've been type-checked. 



---

_Label `red-knot` added by @MichaReiser on 2024-11-02 17:18_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-11-02 17:18_

---

_Comment by @MichaReiser on 2024-11-04 08:17_

We should also remove `ScopeId` from `ClassType` because it has a reference to `node`.

Another approach to removing `Definition` and `ScopeId` (which both seem useful) is to decouple `Definition` and `ScopeId` from the AST node. That seems like a better solution to avoid similar foot-guns in the future.

---

_Closed by @MichaReiser on 2024-11-05 08:02_

---
