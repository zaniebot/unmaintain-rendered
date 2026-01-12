```yaml
number: 321
title: Missing arguments for kwargs
type: issue
state: closed
author: Ryang20718
labels: []
assignees: []
created_at: 2025-05-11T19:02:30Z
updated_at: 2025-05-11T19:05:15Z
url: https://github.com/astral-sh/ty/issues/321
synced_at: 2026-01-12T15:54:22Z
```

# Missing arguments for kwargs

---

_@Ryang20718_

### Summary

https://play.ty.dev/f6f19cc4-3e6e-41f6-a89d-babe60d4860b

In this simple example, extra_fields contains the proper extra fields, but ty complains of `No arguments provided for required parameters`

```
def testing_a(a:str, b:str, c:str):
    print(a, b, c)

def testing(a: str, **extra_fields):
    testing_a("a", **extra_fields)

testing("a", b="b", c="c")
```

Mypy doesn't complain on this, but it doesn't properly raise errors in the case of failure

### Version

ty 0.0.0-alpha.8

---

_Closed by @Ryang20718 on 2025-05-11 19:05_

---
