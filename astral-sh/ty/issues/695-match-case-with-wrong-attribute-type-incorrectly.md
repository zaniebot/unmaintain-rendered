```yaml
number: 695
title: match case with wrong attribute type incorrectly matches
type: issue
state: closed
author: JelleZijlstra
labels:
  - narrowing
  - type-inference
assignees: []
created_at: 2025-06-24T13:09:21Z
updated_at: 2025-07-25T07:13:23Z
url: https://github.com/astral-sh/ty/issues/695
synced_at: 2026-01-10T02:06:24Z
```

# match case with wrong attribute type incorrectly matches

---

_Issue opened by @JelleZijlstra on 2025-06-24 13:09_

### Summary

Example:

```python
from typing import assert_never

class X:
    a: int

def f(x: X) -> int:
    match x:
        case X(a=str() as a):
            return a
        case _:
            assert_never(x)
```

ty accepts this program without errors; mypy and pyright notice that the first case can never match, and therefore error on the `assert_never()` call. (Pyright additionally emits a diagnostic saying that the case can never match; this is nice but not essential.)

There seem to be some Todo types involves so you may already know about this, but I didn't find any similar issues in the tracker.

### Version

_No response_

---

_Comment by @sharkdp on 2025-06-24 13:12_

Thank you for reporting this. I don't think we have an issue that tracks this already.

> There seem to be some Todo types

Yes, this seems to be the TODO here:

https://github.com/astral-sh/ruff/blob/fd2cc37f9055196ef8bd8b573d450fbfa1bd6390/crates/ty_python_semantic/src/types/infer.rs#L3067

---

_Label `narrowing` added by @sharkdp on 2025-06-24 13:13_

---

_Label `type-inference` added by @sharkdp on 2025-06-24 13:13_

---

_Comment by @carljm on 2025-07-24 21:56_

It looks like this was fixed by @sharkdp 's recent fixes to match patterns: https://play.ty.dev/ae08c5da-3e02-4f93-a8f0-afeca6a3f347

---

_Closed by @carljm on 2025-07-24 21:56_

---

_Comment by @sharkdp on 2025-07-25 07:13_

That `@Todo` type can still be observed, though. I opened a new ticket to track pattern matching features that we don't implement yet: https://github.com/astral-sh/ty/issues/887

---
