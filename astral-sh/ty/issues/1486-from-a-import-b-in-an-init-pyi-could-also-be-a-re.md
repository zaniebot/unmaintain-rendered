```yaml
number: 1486
title: "`from .a import b` in an `__init__.pyi` could also be a re-export of `b`"
type: issue
state: closed
author: Gankra
labels:
  - imports
assignees: []
created_at: 2025-11-05T17:23:45Z
updated_at: 2025-11-11T20:09:05Z
url: https://github.com/astral-sh/ty/issues/1486
synced_at: 2026-01-10T02:06:25Z
```

# `from .a import b` in an `__init__.pyi` could also be a re-export of `b`

---

_Issue opened by @Gankra on 2025-11-05 17:23_

This is a followup to https://github.com/astral-sh/ruff/pull/21173 and pertains to behaviours described in [imports/nonstandard_conventions.md](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md)

Currently we consider `from . import b` in an `__init__.pyi` a re-export idiom (equivalent to `from . import b as b`).

We could generalize this to `from .a import b` as well, although that's a bit less explicit.

---

_Assigned to @Gankra by @Gankra on 2025-11-05 17:23_

---

_Label `imports` added by @Gankra on 2025-11-05 17:23_

---

_Comment by @Gankra on 2025-11-11 20:06_

I tried this in https://github.com/astral-sh/ruff/pull/21390 and... it actually had basically 0 impact? Just two (2) accesses of private things that the stubs were probably trying not to expose. Let's Not Do This.

---

_Closed by @Gankra on 2025-11-11 20:06_

---

_Reopened by @carljm on 2025-11-11 20:08_

---

_Closed by @carljm on 2025-11-11 20:09_

---
