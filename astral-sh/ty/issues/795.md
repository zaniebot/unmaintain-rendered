```yaml
number: 795
title: "Type narrowing with `assert x is not None` does not work with `global`"
type: issue
state: closed
author: thorbenk
labels:
  - narrowing
assignees: []
created_at: 2025-07-10T07:30:07Z
updated_at: 2025-07-10T07:34:29Z
url: https://github.com/astral-sh/ty/issues/795
synced_at: 2026-01-10T02:07:36Z
```

# Type narrowing with `assert x is not None` does not work with `global`

---

_Issue opened by @thorbenk on 2025-07-10 07:30_

### Summary

In the following snippet, while the `assert x is not None` works for function `f`, the same does not work for `g`, and I get and unexpected diagnostic:

```python
def f(x: int | None) -> int:
    assert x is not None
    return x


y: int | None = None


def g() -> int:
    global y
    assert y is not None
    return y


print(f(42))
y = 43
print(g())
```

```
uv run ty check tytest.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-return-type]: Return type does not match returned value
  --> tytest.py:9:12
   |
 9 | def g() -> int:
   |            --- Expected `int` because of return type
10 |     global y
11 |     assert y is not None
12 |     return y
   |            ^ expected `int`, found `int | None`
   |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

`mypy` doesn't report any diagnostics here.

I'm running `ty 0.0.1-alpha.14`.

### Version

ty 0.0.1-alpha.14

---

_Label `narrowing` added by @sharkdp on 2025-07-10 07:33_

---

_Comment by @sharkdp on 2025-07-10 07:34_

Thank you for reporting this. I believe this is a duplicate of #311 

---

_Closed by @sharkdp on 2025-07-10 07:34_

---
