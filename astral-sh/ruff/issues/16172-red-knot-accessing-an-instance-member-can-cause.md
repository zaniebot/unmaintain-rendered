```yaml
number: 16172
title: "[red-knot] accessing an instance member can cause cross-module AST dependency"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-02-14T22:56:20Z
updated_at: 2025-02-20T17:46:46Z
url: https://github.com/astral-sh/ruff/issues/16172
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] accessing an instance member can cause cross-module AST dependency

---

_Issue opened by @carljm on 2025-02-14 22:56_

### Description

Currently there is no cached Salsa query in the `Type::member` -> `Class::instance_member` -> `Class::own_instance_member` call chain. The former is public API on `Type` that could be called from any module, and the latter uses the AST from the module where the class is defined. This means that any use of an instance member can cause a direct dependency from one module to another module's AST, which we must avoid, as it will cause invalidation of anything using that instance member type, even if something totally unrelated changes in the source module.

We need to introduce a Salsa query somewhere in that chain to avoid this. And we should add an incremental test to verify the fix.

(Originally observed by @mishamsk in https://github.com/astral-sh/ruff/pull/16036)

---

_Label `red-knot` added by @carljm on 2025-02-14 22:56_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-02-14 22:56_

---

_Closed by @MichaReiser on 2025-02-20 17:46_

---
