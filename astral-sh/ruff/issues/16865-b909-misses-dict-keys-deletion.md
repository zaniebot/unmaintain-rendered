```yaml
number: 16865
title: B909 misses dict.keys() deletion
type: issue
state: open
author: skairunner
labels:
  - rule
  - preview
assignees: []
created_at: 2025-03-20T09:52:03Z
updated_at: 2025-03-20T20:18:10Z
url: https://github.com/astral-sh/ruff/issues/16865
synced_at: 2026-01-12T15:54:55Z
```

# B909 misses dict.keys() deletion

---

_@skairunner_

### Summary

Using this file:

```python
import typing as t

d: t.Dict[str, int] = {
    "a": 1,
    "b": 2,
}

for key in d.keys():
    del d[key]
```

When I run this command:

```
ruff check --preview --select B909 testruff.py
```

I expect to see a B909 failure, but instead all tests pass. I feel like B909 should be able to catch this, especially because running this code raises a RuntimeError:

```
RuntimeError: dictionary changed size during iteration
```

For what it's worth, `flake8-bugbear` also fails to catch this problem.

Modifying the test code to this causes Ruff to correctly point out the B909 error:

```python
import typing as t

d: t.Dict[str, int] = {
    "a": 1,
    "b": 2,
}

for key in d:
    del d[key]
```

### Version

ruff 0.11.0 (2cd25ef64 2025-03-14)

---

_Renamed from "B909 misses dict deletion" to "B909 misses dict.keys() deletion" by @skairunner on 2025-03-20 10:05_

---

_Label `rule` added by @dylwil3 on 2025-03-20 10:23_

---

_Label `preview` added by @dylwil3 on 2025-03-20 10:23_

---

_Comment by @dylwil3 on 2025-03-20 10:24_

That seems like a reasonable extension of the rule to me, thanks!

---

_Comment by @dylwil3 on 2025-03-20 15:24_

We should also add support for detecting `.items` as in:

```python
for k,v in d.items():
    if cond(v):
        del d[k]
```

---

_Comment by @skairunner on 2025-03-20 20:14_

Ultimately at least for dicts, the problem is deleting any of its keys while iterating over a dictionary:

```
for <anything> in <some kind of dictionary iterator over d>:
  #...
  del d[<literally anything>]
```

The deletion doesn't even have to use a value that comes from the iterator, because **all** deletes are illegal. I don't know if it's difficult to encode this into Ruff, though.

Things that can go in the dictionary iterator slot include:
```
# Basic iterators
d
d.keys()
d.items()
# Iteration tools
enumerate(<any of the above>)
zip(<any of the above>, ...)
(most of the itertools functions)
```
The basic iterators and enumerate are probably the most common, though.

---

_Closed by @skairunner on 2025-03-20 20:14_

---

_Reopened by @skairunner on 2025-03-20 20:18_

---
