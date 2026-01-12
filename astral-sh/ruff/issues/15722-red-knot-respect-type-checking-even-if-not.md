```yaml
number: 15722
title: "[red-knot] respect `TYPE_CHECKING` even if not imported from `typing`"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-24T16:04:02Z
updated_at: 2025-03-04T15:58:31Z
url: https://github.com/astral-sh/ruff/issues/15722
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] respect `TYPE_CHECKING` even if not imported from `typing`

---

_@carljm_

### Description

In order to avoid the runtime cost of importing `typing` module, some users use a pattern like this:

```py
TYPE_CHECKING = False
if TYPE_CHECKING:
    from typing import ...
```

Where the type checker should recognize the name `TYPE_CHECKING` and treat it as `True` for type checking. Mypy and pyright both support this.

This is of course a bit ugly because we're recognizing an arbitrary name wherever it's used, but in practice the name is so distinctive that this doesn't seem to cause a lot of trouble.

It can be supported in a general way with a config option that allows specifying arbitrary names that should always be considered as builtin constants with some type, but this isn't usable for a library that can't control its users' type checker configuration.

Ideally we would specify a more principled way to do this (though that might also not be practically worth the community churn.)

But the use case is valid, and we should support it the same way pyright and mypy do, to avoid adding a migration barrier.

---

_Label `red-knot` added by @carljm on 2025-01-24 16:04_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-24 16:04_

---

_Comment by @brandonsorensen on 2025-02-11 11:54_

Can this issue be closed now that #15719 is merged?

---

_Comment by @MichaReiser on 2025-02-11 12:38_

This issue is for red-knot, our new type checker. It's separate from Ruff.

---

_Closed by @carljm on 2025-03-04 15:58_

---

_Closed by @carljm on 2025-03-04 15:58_

---
