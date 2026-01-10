```yaml
number: 359
title: "Inconsistent casing in \"Expected\" vs. \"expected\""
type: issue
state: closed
author: charliermarsh
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-05-13T15:19:56Z
updated_at: 2025-05-14T06:27:16Z
url: https://github.com/astral-sh/ty/issues/359
synced_at: 2026-01-10T02:34:09Z
```

# Inconsistent casing in "Expected" vs. "expected"

---

_Issue opened by @charliermarsh on 2025-05-13 15:19_

E.g., "Expected":

```
Return type does not match returned value: Expected `Sequence[Cursor]`, found `list[_T]`
```

Versus "expected":

```
Too many positional arguments to bound method `__init__`: expected 0, got 1
```

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-13 15:20_

---

_Label `help wanted` added by @carljm on 2025-05-13 15:30_

---

_Comment by @kiran-4444 on 2025-05-13 17:27_

I’d like to take this up.

---

_Assigned to @kiran-4444 by @AlexWaygood on 2025-05-13 17:39_

---

_Comment by @MichaReiser on 2025-05-14 06:27_

Fixed in https://github.com/astral-sh/ruff/pull/18084

---

_Closed by @MichaReiser on 2025-05-14 06:27_

---
