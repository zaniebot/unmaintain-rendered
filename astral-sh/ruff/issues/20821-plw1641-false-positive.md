```yaml
number: 20821
title: "`PLW1641`: false positive"
type: issue
state: closed
author: chirizxc
labels:
  - question
assignees: []
created_at: 2025-10-12T19:05:48Z
updated_at: 2025-12-10T17:30:14Z
url: https://github.com/astral-sh/ruff/issues/20821
synced_at: 2026-01-12T15:54:57Z
```

# `PLW1641`: false positive

---

_@chirizxc_

The current [`eq-without-hash` (PLW1641)](https://docs.astral.sh/ruff/rules/eq-without-hash/) rule warns whenever a class defines `__eq__` without defining `__hash__`. However, this creates false positives for legitimate use cases.

Not all classes with `__eq__` are intended to be used as dictionary keys or set members. For mutable objects, having `__hash__ = None` is actually the correct behavior according to [Python's data model](https://docs.python.org/3/reference/datamodel.html):
<img width="1078" height="161" alt="Image" src="https://github.com/user-attachments/assets/49dd2b7e-9d3d-4812-80a6-36fa032004c4" />

Consider the inverse check instead: warn when a class defines `__hash__` (and it's not None) but doesn't define `__eq__`. This would catch cases where the hash/equality contract is violated: objects that are equal must have the same hash value.


[Playground*](https://play.ruff.rs/109fab87-5be0-47e9-8c17-324ab18ec3db)

<img width="703" height="451" alt="Image" src="https://github.com/user-attachments/assets/70d7c616-8dfc-4a7a-add7-6709d7791899" />

---

_Renamed from "Improve rule eq-without-hash (PLW1641)" to "`PLW1641`: false positive" by @chirizxc on 2025-10-13 10:59_

---

_Comment by @ntBre on 2025-10-13 13:05_

I think this might be a duplicate of, or at least closely related to, https://github.com/astral-sh/ruff/issues/18821. As noted there, I don't think there's a good way for us to detect a mutable object or otherwise suppress the error. Explicitly setting `__hash__ = None` seems like the easiest way to indicate your intention to Ruff. [This comment](https://github.com/astral-sh/ruff/issues/18821#issuecomment-3185232691) mentions that this could lead to other linting errors, but I'm not seeing any on the [playground](https://play.ruff.rs/7447950f-a7ab-4ad6-afcf-c1e7fca5f302), at least with my config there.

---

_Label `question` added by @ntBre on 2025-10-13 13:05_

---

_Closed by @MichaReiser on 2025-12-10 17:30_

---
