---
number: 5909
title: "`RET504` invalid autofix"
type: issue
state: closed
author: tylerlaprade
labels:
  - bug
assignees: []
created_at: 2023-07-20T08:55:36Z
updated_at: 2024-01-30T22:00:09Z
url: https://github.com/astral-sh/ruff/issues/5909
synced_at: 2026-01-10T01:22:44Z
---

# `RET504` invalid autofix

---

_Issue opened by @tylerlaprade on 2023-07-20 08:55_

```
def foo(data):
    with contextlib.suppress(UnicodeDecodeError):
        data = data.decode()
    return data
```
is autofixed to
```
def foo(data):
    with contextlib.suppress(UnicodeDecodeError):
        return data.decode()
```
These are not equivalent. The first one will always return `data`, either decoded or the original. The second will return the decoded data if there's no exception, otherwise, it will return `None`. This introduced a bug into my code that I only caught from some miscellaneous Pyright type-checking far away from the autofix.

---

_Comment by @zanieb on 2023-07-20 13:33_

Thanks for the report @tylerlaprade. It looks like perhaps we should not raise RET504 inside context managers?

---

_Label `bug` added by @zanieb on 2023-07-20 13:34_

---

_Comment by @dhruvmanila on 2023-07-20 13:45_

I think this is a special case as `data` is defined outside (before) the context manager and/or that the `contextlib.suppress` context manager is used here.

---

_Comment by @zanieb on 2023-07-20 13:49_

@dhruvmanila the problem is that _any_ context manager could suppress exceptions, we can't really know. I agree that `data` is defined before the context is important as well though.

---

_Comment by @charliermarsh on 2023-07-20 14:08_

Oh jeez what an interesting case.

---

_Comment by @tylerlaprade on 2023-07-20 23:58_

Sourcery has been autofixing all my `try-catch-pass` blocks to the more concise `contextlib.suppress` (which I'm happy about!), but it opens up potential opportunities for this bug.

---

_Comment by @charliermarsh on 2023-07-21 00:00_

Shameless plug that Ruff can do it too :joy: https://beta.ruff.rs/docs/rules/suppressible-exception/

---

_Comment by @tylerlaprade on 2023-07-21 03:02_

Ah, I do have `SIM` enabled already, so I guess I have two sources of the autofixes!

---

_Referenced in [astral-sh/ruff#9673](../../astral-sh/ruff/pulls/9673.md) on 2024-01-29 12:47_

---

_Comment by @tylerlaprade on 2024-01-30 22:00_

This appears to be fixed in `0.1.15` so I will close this, thanks!

---

_Closed by @tylerlaprade on 2024-01-30 22:00_

---
