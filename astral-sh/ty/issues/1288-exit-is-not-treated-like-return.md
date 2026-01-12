```yaml
number: 1288
title: exit() is not treated like return
type: issue
state: closed
author: markwaddle
labels: []
assignees: []
created_at: 2025-09-30T16:54:55Z
updated_at: 2025-10-01T18:42:46Z
url: https://github.com/astral-sh/ty/issues/1288
synced_at: 2026-01-12T15:54:24Z
```

# exit() is not treated like return

---

_@markwaddle_

### Summary

the following results in invalid_argument_type on the final line. it should treat the exit like a return - that's what `pyright` does by default. it seems like the correct thing to me.

`ty` settings are all defaults.

```python
def f_str(v: str) -> str:
    return v

def x_val() -> str | None:
    return None

def f():
    x = x_val()
    if not x or x is None: 
        exit(1)
    f_str(x)
```

### Version

_No response_

---

_Comment by @sharkdp on 2025-09-30 17:09_

Thank you for reporting this.


This will be solved by #690. See similar reports here, for example: https://github.com/astral-sh/ty/issues/1110

---

_Closed by @sharkdp on 2025-09-30 17:09_

---

_Comment by @markwaddle on 2025-10-01 18:42_

thanks @sharkdp. i looked for an existing issue, but couldn't find it.

---
