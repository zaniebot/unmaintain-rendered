```yaml
number: 1484
title: "`from` imports shouldn't have to be relative to introduce submodule locals"
type: issue
state: closed
author: Gankra
labels:
  - bug
  - imports
assignees: []
created_at: 2025-11-05T16:58:34Z
updated_at: 2025-11-11T18:04:43Z
url: https://github.com/astral-sh/ty/issues/1484
synced_at: 2026-01-10T02:06:25Z
```

# `from` imports shouldn't have to be relative to introduce submodule locals

---

_Issue opened by @Gankra on 2025-11-05 16:58_

This is a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

`from .a import b` in an `__init__.pyi` file introduces `a` as a local (via `ImportFromImplicitDefinitionNodeRef`). 

We should also do this for `from whatever.thispackage.a import b`. This is "simply" identifying that current module is `whatever.thispackage` in semantic index building. Hopefully that "simply" is in fact simple.

---

_Assigned to @Gankra by @Gankra on 2025-11-05 16:58_

---

_Label `bug` added by @Gankra on 2025-11-05 16:58_

---

_Label `imports` added by @Gankra on 2025-11-05 16:58_

---

_Closed by @Gankra on 2025-11-11 18:04_

---
