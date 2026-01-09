---
number: 12486
title: Default for dict.get(). Possible new rule
type: issue
state: open
author: ashrub-holvi
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-07-24T07:35:22Z
updated_at: 2024-07-25T06:41:03Z
url: https://github.com/astral-sh/ruff/issues/12486
synced_at: 2026-01-07T13:12:15-06:00
---

# Default for dict.get(). Possible new rule

---

_Issue opened by @ashrub-holvi on 2024-07-24 07:35_

Hi,
I am not fully sure, so, it's more about starting discussion, but have a feeling it might be good to have a rule for such cases.
There is a standard way to get default while getting key's value from a dictionary:
```
mydict.get("key_name", default_value)
```
and it works fine in most of the cases, but it's easy to forget that it will set default only if key is not present in dict. If key is present and have some empty value like `None` get() will not return default:
```
>>> mydict = dict(key=None)
>>> print(mydict.get("key", 0))
None
```
It's correct and [documented](https://docs.python.org/3/library/stdtypes.html#dict.get) behaviour, but can confuse, sometimes correct way is:
```
mydict.get("key") or 0
```
but probably linter can't know which way correct in particular cases.

---

_Label `rule` added by @MichaReiser on 2024-07-24 07:36_

---

_Label `type-inference` added by @MichaReiser on 2024-07-24 07:36_

---

_Comment by @scastlara on 2024-07-25 06:20_

How would a linter like ruff know which one you intended to write? Both are correct and semantically different.

---

_Comment by @ashrub-holvi on 2024-07-25 06:41_

> How would a linter like ruff know which one you intended to write? Both are correct and semantically different.

As I mentioned - probably there is no way to know it, but perhaps linter can always suggest `mydict.get("key") or 0` as it's what most of people would expect. And for other cases, where developer want to set default only if key if not present, linter can suggest:
```
if key_name is not in mydict:
    # use default
```
as it's much more explicit.

---
