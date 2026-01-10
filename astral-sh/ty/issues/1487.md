```yaml
number: 1487
title: "`from thispackage import b` in an `__init__.pyi` could also be a re-export `b`"
type: issue
state: closed
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-05T17:25:43Z
updated_at: 2025-11-11T19:41:15Z
url: https://github.com/astral-sh/ty/issues/1487
synced_at: 2026-01-10T02:06:25Z
```

# `from thispackage import b` in an `__init__.pyi` could also be a re-export `b`

---

_Issue opened by @Gankra on 2025-11-05 17:25_

This is a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

Currently we consider `from . import b` in an `__init__.pyi` a re-export idiom (equivalent to `from . import b as b`).

We could generalize this to `from whatever.thispackage import b` as well, although that's a bit less explicit.

Related to:

* https://github.com/astral-sh/ty/issues/1484

---

_Assigned to @Gankra by @Gankra on 2025-11-05 17:25_

---

_Label `imports` added by @Gankra on 2025-11-05 17:25_

---

_Renamed from "`from thispackage.a import b` could also be a re-export `b`" to "`from thispackage.a import b` in an `__init__.pyi` could also be a re-export `b`" by @Gankra on 2025-11-05 17:59_

---

_Renamed from "`from thispackage.a import b` in an `__init__.pyi` could also be a re-export `b`" to "`from thispackage import b` in an `__init__.pyi` could also be a re-export `b`" by @Gankra on 2025-11-06 04:22_

---

_Closed by @Gankra on 2025-11-11 19:41_

---
