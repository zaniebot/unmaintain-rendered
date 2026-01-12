```yaml
number: 16839
title: F821 (E0601) False negative
type: issue
state: open
author: pa5555
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-03-19T07:01:17Z
updated_at: 2025-03-19T15:08:49Z
url: https://github.com/astral-sh/ruff/issues/16839
synced_at: 2026-01-12T15:54:55Z
```

# F821 (E0601) False negative

---

_@pa5555_

### Summary

Both pylint and ruff fails to detect mistake in the code, see attached link

https://play.ruff.rs/f229c153-a059-4efe-99d5-2a600f0ecc70

### Version

0.11.0

---

_Comment by @ntBre on 2025-03-19 13:36_

Thanks for the report! There are some other open issues around F821 (https://github.com/astral-sh/ruff/issues/6242 and https://github.com/astral-sh/ruff/issues/9858, for example), but those seem to be related to `del`. I think it's worth tracking this separately.

I think we would need to know that the assignment to `x` can raise an exception, so this probably requires better type inference. Or we could take the approach described in the [pylint docs](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/used-before-assignment.html):

> Assignments in try blocks are assumed not to have occurred when evaluating associated except/finally blocks.

(pasting the playground code for easier quick reference)

```python
# ruff says all good
# pylint says: E0601: Using variable 'x' before assignment (used-before-assignment)
try:
    x = 1 / 0
    #if x:
    #    ...
except Exception:
    print(x)


# ruff says all good
# pylint says all good
try:
    x = 1 / 0
    if x:
        ...
except Exception:
    print(x)
```

---

_Label `bug` added by @ntBre on 2025-03-19 13:37_

---

_Comment by @MichaReiser on 2025-03-19 15:08_

I think the most correct would be to mark every variable defined inside a `try` to be possibly unbound in the `except` handlers and any code coming after. However, I worry that this is too strict for most users and should probably be its own rule. At least, this is what we do in our new type checker ([playground](https://playknot.ruff.rs/2ac1f0fb-c2b6-477a-bea5-3a6a77edceb7))

---

_Label `type-inference` added by @MichaReiser on 2025-03-19 15:08_

---
