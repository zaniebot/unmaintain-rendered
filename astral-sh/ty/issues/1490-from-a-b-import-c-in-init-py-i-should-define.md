```yaml
number: 1490
title: "`from .a.b import c` in `__init__.py(i)` should define submodule attributes for *all* files"
type: issue
state: open
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-05T17:45:40Z
updated_at: 2025-11-11T21:20:08Z
url: https://github.com/astral-sh/ty/issues/1490
synced_at: 2026-01-12T15:54:25Z
```

# `from .a.b import c` in `__init__.py(i)` should define submodule attributes for *all* files

---

_@Gankra_

This is kind of a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

We do not currently consider `from .a.b import c` in an `__init__.py(i)` as ensuring `thispackage.a.b` or `thispackage.a.b.c` are submodule attributes in files that import `thispackage`, even though logically anything that acquires a reference to that module must have executed that import.

Ideally it should, but I think for safety it requires:

* #1488

(This one may be a bad idea for performance/caching/isolation reasons, but I think it might be ok since we have to load and analyze that package's module anyway when you access an attribute on it?)

---

_Assigned to @Gankra by @Gankra on 2025-11-05 17:45_

---

_Label `imports` added by @Gankra on 2025-11-05 17:45_

---

_Unassigned @Gankra by @Gankra on 2025-11-11 21:20_

---
