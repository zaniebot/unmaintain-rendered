---
number: 8125
title: "`if-with-same-arms` (`SIM114`) false negative on `if` statements that raise or return"
type: issue
state: closed
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2023-10-23T06:47:44Z
updated_at: 2026-01-03T13:50:53Z
url: https://github.com/astral-sh/ruff/issues/8125
synced_at: 2026-01-07T13:12:15-06:00
---

# `if-with-same-arms` (`SIM114`) false negative on `if` statements that raise or return

---

_Issue opened by @DetachHead on 2023-10-23 06:47_

```py
# no error
if bool():
    raise Exception("foo")
if bool():
    raise Exception("foo")


def foo():
    # no error
    if bool():
        return "foo"
    if bool():
        return "foo"
```
https://play.ruff.rs/7890091d-5879-4420-9cf8-72cf9b7728f1

---

_Label `rule` added by @charliermarsh on 2023-10-23 16:20_

---

_Comment by @qdegraaf on 2023-10-25 09:49_

I don't think the issue here is with `Raise` or `return` but the fact that its separate `if` statements and not `if/elif` branches. If I run it against:

```python
if bool():
    raise Exception("foo")
elif bool():
    raise Exception("foo")


def foo():
    if bool():
        return "foo"
    elif bool():
        return "foo"

```
SIM114 raises an error:
```
        163 │+SIM114.py:120:1: SIM114 Combine `if` branches using logical `or` operator
        164 │+    |
        165 │+119 |   # no error
        166 │+120 | / if bool():
        167 │+121 | |     raise Exception("foo")
        168 │+122 | | elif bool():
        169 │+123 | |     raise Exception("foo")
        170 │+    | |__________________________^ SIM114
        171 │+    |
        172 │+
        173 │+SIM114.py:128:5: SIM114 Combine `if` branches using logical `or` operator
        174 │+    |
        175 │+126 |   def foo():
        176 │+127 |       # no error
        177 │+128 |       if bool():
        178 │+    |  _____^
        179 │+129 | |         return "foo"
        180 │+130 | |     elif bool():
        181 │+131 | |         return "foo"
        182 │+    | |____________________^ SIM114
        183 │+    |
        184 │+
```

Looking at upstream implementation it doesn't look like that catches identical separate `if` statements either. So I don't think this is a bug. Is this something we want to check for in SIM114 or a separate rule? 

---

_Comment by @dhruvmanila on 2023-11-01 05:55_

Thanks @qdegraaf for taking the time to look into this issue!

---

_Comment by @charliermarsh on 2024-04-12 02:12_

I think the current behavior is reasonable.

---

_Closed by @charliermarsh on 2024-04-12 02:12_

---

_Comment by @GideonBear on 2026-01-03 13:11_

I would like to ask for reconsideration, especially because of the existence of [superfluous-else-return (RET505)](https://docs.astral.sh/ruff/rules/superfluous-else-return/#superfluous-else-return-ret505). RET505 indicates that writing this in the `if-if` style is preferred to writing it in the `if-elif` style. Take the following example: [playground](https://play.ruff.rs/a1efcc7f-af3e-4d6b-a923-c958ea86bf1a)

The functions have exactly the same behavior, and in both, SIM114 is valid. But by writing it in the preferred style following RET505, SIM114 doesn't trigger anymore. This means that people that write in the RET505 style by themselves, or fix RET505 before SIM114, don't get the benefit of SIM114.

I propose making SIM114 check, similarly to what RET505 checks in the first block, if the blocks return in all paths. If that is the case, pretend like it is `if-elif`.

This will make the rule more complex, as it will need to run on all pairs/sets of if statements. This can be a new RUF rule as well, if you don't want to overload SIM114.

---
