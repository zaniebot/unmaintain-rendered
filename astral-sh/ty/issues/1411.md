```yaml
number: 1411
title: "Improve `invalid-argument` diagnostic for union types"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-10-22T15:59:57Z
updated_at: 2025-10-23T13:16:23Z
url: https://github.com/astral-sh/ty/issues/1411
synced_at: 2026-01-10T02:06:25Z
```

# Improve `invalid-argument` diagnostic for union types

---

_Issue opened by @MichaReiser on 2025-10-22 15:59_

The `invalid-argument` diagnostic doesn't contain enough information for me to fix the diagnostic if the union gets truncated:


```py
def arg(a: list[str] | str | dict[str, str] | list[int]| set[str] | int):
    assert len(a) == 1, (
        "ensure_config should not modify the original config"
    )

```

```
Argument to function `len` is incorrect: Expected `Sized`, found `list[str] | str | dict[str, str] | ... omitted 3 union elements` (invalid-argument-type) [Ln 2, Col 16]
```

The problematic element is `int` but it doesn't appear in the list. 

We should improve the `invalid-argument` diagnostic to explicitly call out the argument that isn't compatible. Not only does that fix the issue with truncated union types, it also removes the need for users to second-guess which union element is the problematic one.

https://play.ty.dev/0ef049ec-72e9-4700-9fca-bfc1a45f965d

Related discussion https://github.com/astral-sh/ruff/pull/20730#discussion_r2410446248

---

_Label `help wanted` added by @MichaReiser on 2025-10-22 15:59_

---

_Label `diagnostics` added by @MichaReiser on 2025-10-22 15:59_

---

_Closed by @AlexWaygood on 2025-10-23 13:16_

---
