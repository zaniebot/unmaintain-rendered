---
number: 10948
title: "misleading `E721 Do not compare types, use isinstance()` for bool"
type: issue
state: closed
author: Borda
labels:
  - rule
assignees: []
created_at: 2024-04-15T07:51:37Z
updated_at: 2024-05-02T18:02:13Z
url: https://github.com/astral-sh/ruff/issues/10948
synced_at: 2026-01-07T13:12:15-06:00
---

# misleading `E721 Do not compare types, use isinstance()` for bool

---

_Issue opened by @Borda on 2024-04-15 07:51_

just crossed [this case](https://github.com/unit8co/darts/pull/2323#discussion_r1565286329) where for `type(component_wise) is bool` ruff suggest generic `isinstance(component_wise, bool)` which is not the same as this comparison takes int as bool:
```py
>>> isinstance(5, bool)
True
>>> type(5) is bool
False
```
see why this si happening in https://stackoverflow.com/a/37888668/4521646


---

_Referenced in [unit8co/darts#2323](../../unit8co/darts/pulls/2323.md) on 2024-04-15 07:51_

---

_Label `rule` added by @MichaReiser on 2024-04-15 08:11_

---

_Comment by @mscheifer on 2024-04-26 20:45_

I think your example is backwards. `bool`s are `int`s but not the other way around.
```python
>>> isinstance(5, bool)
False
>>> isinstance(True, int)
True
```

---

_Comment by @dhruvmanila on 2024-04-30 08:36_

Yeah, I think the behavior is correct?

```
>>> isinstance(5, bool)
False
>>> type(5) is bool
False
```

As per the linked stackoverflow question, I think the problem would be when you compare against an `int` where the behavior is different:
```
>>> isinstance(5, int)
True
>>> isinstance(True, int)
True
>>> type(5) is int
True
>>> type(True) is int
False
```

Also, this is already resolved in `--preview`, refer https://github.com/astral-sh/ruff/pull/7905. It should available generally in the next minor bump.

---

_Closed by @charliermarsh on 2024-05-02 18:01_

---

_Comment by @charliermarsh on 2024-05-02 18:02_

Closing based on @dhruvmanila's analysis.

---
