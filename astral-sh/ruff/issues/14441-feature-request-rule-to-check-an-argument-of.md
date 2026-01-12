```yaml
number: 14441
title: "Feature request: rule to check an argument of `pathlib.Path().with_suffix`"
type: issue
state: closed
author: TheBits
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-18T19:48:45Z
updated_at: 2024-12-06T12:08:21Z
url: https://github.com/astral-sh/ruff/issues/14441
synced_at: 2026-01-12T15:54:53Z
```

# Feature request: rule to check an argument of `pathlib.Path().with_suffix`

---

_@TheBits_

Hi!

The [argument of function `pathlib.Path().with_suffix`](https://github.com/python/cpython/blob/v3.13.0/Lib/pathlib/_abc.py#L231) must starts with a dot. It's easy to forget if you don't use it often. Can you please add a new rule to check the argument?

Thanks!

---

_Label `rule` added by @dylwil3 on 2024-11-18 22:37_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-18 22:37_

---

_Closed by @MichaReiser on 2024-12-06 12:08_

---
