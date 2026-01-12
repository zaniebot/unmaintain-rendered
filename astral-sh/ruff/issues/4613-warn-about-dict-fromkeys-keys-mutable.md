```yaml
number: 4613
title: "Warn about `dict.fromkeys(keys, mutable)`"
type: issue
state: closed
author: janosh
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-05-23T21:21:12Z
updated_at: 2024-01-22T00:22:03Z
url: https://github.com/astral-sh/ruff/issues/4613
synced_at: 2026-01-12T15:54:44Z
```

# Warn about `dict.fromkeys(keys, mutable)`

---

_@janosh_

`ruff` should error and suggest the following fix when passing a mutable 2nd arg to `dict.fromkeys()` since modifications will affect all keys which is a common pitfall.

```py
# bad
dict.fromkeys((1, 2), {})
dict.fromkeys((1, 2), [])
dict.fromkeys((1, 2), set())
# ...

# good
{key: {} for key in (1, 2)}
{key: [] for key in (1, 2)}
{key: set() for key in (1, 2)}
```

Examples:
- [StackoverFlow](https://stackoverflow.com/q/11509721)
- [SO 2](https://stackoverflow.com/q/8174723)
- [commit](https://github.com/CederGroupHub/chgnet/commit/c8ba05443280c280f904a7076f4635d3326bac66#diff-a7bc3c3838885a61fa9d82f50027518237bf263cbbcd33781f1176b7031f1275R184-R186)



---

_Label `rule` added by @charliermarsh on 2023-05-24 02:07_

---

_Comment by @charliermarsh on 2023-05-24 02:07_

Oh interesting, I've never seen that!

---

_Renamed from "Error on `dict.fromkeys(keys, mutable)`" to "Warn about `dict.fromkeys(keys, mutable)`" by @janosh on 2023-05-24 19:50_

---

_Comment by @Skylion007 on 2023-05-28 14:58_

Flake8-bugbear might be interested in adding this rule too, you should open an issue there as well.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:29_

---

_Label `needs-decision` removed by @charliermarsh on 2024-01-20 15:35_

---

_Label `accepted` added by @charliermarsh on 2024-01-20 15:35_

---

_Comment by @charliermarsh on 2024-01-20 15:35_

Marking as accepted, makes sense to me.

---

_Comment by @tjkuson on 2024-01-20 20:30_

I'll implement this tomorrow (as a precursor to #9592)!

---

_Closed by @charliermarsh on 2024-01-22 00:22_

---
