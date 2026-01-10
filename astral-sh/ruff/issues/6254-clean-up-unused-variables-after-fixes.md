---
number: 6254
title: Clean up unused variables after fixes
type: issue
state: open
author: tylerlaprade
labels:
  - fixes
assignees: []
created_at: 2023-08-01T17:44:18Z
updated_at: 2023-08-01T21:34:59Z
url: https://github.com/astral-sh/ruff/issues/6254
synced_at: 2026-01-10T01:22:45Z
---

# Clean up unused variables after fixes

---

_Issue opened by @tylerlaprade on 2023-08-01 17:44_

```python
x = None
try:
    x = func_that_might_throw()
except AttributeError:
    pass
return x
```
is getting simplified to
```
x = None  # Variable "x" is not accessed
with contextlib.suppress(AttributeError):
    return func_that_might_throw()
```
If this is the first time we've seen `x`, can we clean up the null assignment?

---

_Label `autofix` added by @zanieb on 2023-08-01 17:49_

---

_Label `accepted` added by @zanieb on 2023-08-01 17:49_

---

_Comment by @zanieb on 2023-08-01 17:50_

Thanks for the report! Seems reasonable to me although it may be non-trivial.

---

_Comment by @charliermarsh on 2023-08-01 17:53_

I believe this works as expected, assuming you enable the right set of rules: `ruff check foo.py --select SIM --select RET --select F -n --fix`.

We like to treat each fix as independent. Here, the `SIM105` rule gives you:

```python
import contextlib
def f():
    x = None
    with contextlib.suppress(AttributeError):
        x = func_that_might_throw()

    return x
```

Then the `RET` rule simplifies to:

```python
import contextlib
def f():
    x = None
    with contextlib.suppress(AttributeError):
        return func_that_might_throw()
```

Then the `F` rule identifies the unused variable:

```python
import contextlib
def f():
    with contextlib.suppress(AttributeError):
        return func_that_might_throw()
```

(This all happens in one `ruff` invocation, I'm just breaking it down for clarity.)

Perhaps you have `F841` disabled?


---

_Comment by @tylerlaprade on 2023-08-01 18:29_

Ahh, @charliermarsh is correct.
```toml
unfixable = ["B007", "ERA001", "F841", "RUF100"]
```
We disabled autofix on `F841` because variables are often temporarily unused while rearranging a file. We don't want an errant `⌘ + S` to accidentally delete useful code. In this case, though, there's no danger since it's only unused as a result of the other (safe) autofixes.

---

_Comment by @zanieb on 2023-08-01 18:58_

@tylerlaprade I agree that seems like a fair use-case. We probably don't want to resolve this _just_ for this specific rule though, instead we should work on a general solution that checks for unused variables _introduced_ by an autofix.

Would you mind if I retitle this issue?



---

_Comment by @tylerlaprade on 2023-08-01 19:05_

Yes, please do! I'm glad this could potentially turn out to be more generally applicable than just a single rule.

---

_Renamed from "`SIM105` should clean up unused variable after refactor" to "Clean up unused variables after fixes" by @zanieb on 2023-08-01 19:09_

---

_Comment by @zanieb on 2023-08-01 19:10_

In the meantime, for this use-case, I'd recommend leaving `F841` enabled in your configuration and only disabling it in the flags passed when used by the IDE. Would that help?

---

_Label `accepted` removed by @zanieb on 2023-08-01 19:26_

---

_Comment by @tylerlaprade on 2023-08-01 20:55_

Yes, that's a good suggestion, thank you Zanie!

---

_Comment by @tylerlaprade on 2023-08-01 21:01_

Ah, no luck - that same flag is used for the recursive fixes, so `⌘ + S` still has the same behavior.

---

_Comment by @charliermarsh on 2023-08-01 21:34_

I think the suggestion was to leave you with a setup such that unused variables would be removed when running via the CLI, but not when running in the editor (which could happen "by accident").

---

_Referenced in [astral-sh/ruff#10954](../../astral-sh/ruff/issues/10954.md) on 2024-04-15 12:46_

---
