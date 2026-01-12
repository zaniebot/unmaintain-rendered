```yaml
number: 8220
title: "`ARG0xx`: Miscounted arguments"
type: issue
state: closed
author: smurfix
labels: []
assignees: []
created_at: 2023-10-25T14:26:19Z
updated_at: 2023-10-25T16:09:45Z
url: https://github.com/astral-sh/ruff/issues/8220
synced_at: 2026-01-12T15:54:47Z
```

# `ARG0xx`: Miscounted arguments

---

_@smurfix_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```
def foo(*a, **k):
    return "Duh"
```

This reports:
```
test.py:1:10: ARG001 Unused function argument: `a`
test.py:1:15: ARG001 Unused function argument: `k`
```
Well, duh, except that `k` is argument 2.

ruff 0.1.1

---

_Comment by @T-256 on 2023-10-25 14:37_

Hello @smurfix
What's your expected result here?

`a` is _positional_ argument
`k` is _keyword_ argument

---

_Comment by @smurfix on 2023-10-25 14:39_

This also happens with "normal" arguments.

---

_Comment by @T-256 on 2023-10-25 14:44_

> This also happens with "normal" arguments.

Yes, I think this is correct as they are all kind of _argument_

---

_Comment by @smurfix on 2023-10-25 14:54_

>  What's your expected result here?

The expected result here is that the errors are numbered `ARG001` and `ARG002`.
This works perfectly when only one argument (`k` in my case) is unused.

---

_Comment by @smurfix on 2023-10-25 15:01_

OK, so it works semi-perfectly.
```python3
    class XPin:
        def __new__(cls, **kw):  # noqa:ARG003
            return object.__new__(cls)
```

Note `ARG003` where I'd expect `ARG002`. 

```python3
class YPin:
    def __init__(self, cfg, **kw):  # noqa:ARG002
        self.t = cfg.get("t", 1000)
```

Note the converse problem.

---

_Comment by @T-256 on 2023-10-25 15:12_

> Note `ARG003` where I'd expect `ARG002`.

`__new__` is class method, so `ARG003` is correct there.

Ref:
ARG002: [unused-method-argument](https://docs.astral.sh/ruff/rules/unused-method-argument/)
ARG003: [unused-class-method-argument](https://docs.astral.sh/ruff/rules/unused-class-method-argument/)


---

_Comment by @smurfix on 2023-10-25 16:09_

Owch. My bad.

---

_Closed by @smurfix on 2023-10-25 16:09_

---
