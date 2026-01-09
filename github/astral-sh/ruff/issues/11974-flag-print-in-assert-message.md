---
number: 11974
title: "flag `print` in `assert` message"
type: issue
state: closed
author: janosh
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-06-21T18:21:04Z
updated_at: 2024-06-23T17:30:41Z
url: https://github.com/astral-sh/ruff/issues/11974
synced_at: 2026-01-07T13:12:15-06:00
---

# flag `print` in `assert` message

---

_Issue opened by @janosh on 2024-06-21 18:21_

i've come across code like this a few times, leading me to believe a lint rule to catch this makes sense

```py
assert 1 == 2, print("probably didn't mean to print this")
>>> AssertionError: None
```

the developer very likely wanted the `print` message to become the error message but of course the error message is `None` instead, `print`'s return value

---

_Label `rule` added by @zanieb on 2024-06-21 18:50_

---

_Comment by @zanieb on 2024-06-21 18:51_

I thought there was a rule for this but I don't see one. This makes sense to me.

---

_Comment by @denwong47 on 2024-06-21 23:46_

Seeing that this could be a fairly straight forward condition to check against, I would like to have a go at this as Ruff Rule 030.

I wonder if we have to support all variations of `print` calls, such as:
```python
U00A0 = "\u00a0"
assert False, print("Unreachable due to", condition, f", ask {maintainer} for advice", sep=U00A0)
```

I can potentially do a `FStringValue::concatenated` as:
```python
assert False, "Unreachable due to" f"{U00A0}" f"{condition}" f"{U00A0}" f", ask {maintainer} for advice"
```
But honestly it does not make very readable code.

Any advice would be welcomed.

**EDIT**

Looks like I can support the above case with just a `FStringValue::single`. I'll put up a PR tomorrow.

---

_Comment by @MichaReiser on 2024-06-22 07:15_

I wonder if it makes sense to phrase this rule more generally. Overall, it is probably incorrect if anything that returns `None` is passed as the assertion message. But implementing this would likely require type inference.

---

_Comment by @denwong47 on 2024-06-22 09:39_

With type inference this rule could become a nice-to-have: since the return type of print already violates the type of the expression, the only thing that this rule will provide is a nicer error message instead of "str is expected but found NoneType".

---

_Referenced in [astral-sh/ruff#11981](../../astral-sh/ruff/pulls/11981.md) on 2024-06-22 12:22_

---

_Label `accepted` added by @charliermarsh on 2024-06-23 15:01_

---

_Comment by @charliermarsh on 2024-06-23 17:30_

Added by @denwong47 in https://github.com/astral-sh/ruff/pull/11981. Thank you, great contribution!

---

_Closed by @charliermarsh on 2024-06-23 17:30_

---
