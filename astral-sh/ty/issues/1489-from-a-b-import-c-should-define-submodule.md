```yaml
number: 1489
title: "`from a.b import c` should define submodule attributes in the current file"
type: issue
state: open
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-05T17:43:32Z
updated_at: 2025-11-11T21:20:11Z
url: https://github.com/astral-sh/ty/issues/1489
synced_at: 2026-01-12T15:54:25Z
```

# `from a.b import c` should define submodule attributes in the current file

---

_@Gankra_

This is kind of a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

We do not currently consider `from a.b import c` as ensuring `a.b` or `a.b.c` are submodule attributes in the current file, meaning (e.g. if you then `import a` or otherwise get access to that module).

Ideally it should, but I think for safety it requires:

* #1488

---

_Assigned to @Gankra by @Gankra on 2025-11-05 17:43_

---

_Label `imports` added by @Gankra on 2025-11-05 17:43_

---

_Unassigned @Gankra by @Gankra on 2025-11-11 21:20_

---
