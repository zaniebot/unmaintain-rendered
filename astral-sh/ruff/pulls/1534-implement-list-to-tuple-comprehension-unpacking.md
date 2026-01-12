```yaml
number: 1534
title: Implement list-to-tuple comprehension unpacking
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: bytesliteral
created_at: 2023-01-01T19:27:11Z
updated_at: 2023-01-01T23:34:37Z
url: https://github.com/astral-sh/ruff/pull/1534
synced_at: 2026-01-12T05:36:31Z
```

# Implement list-to-tuple comprehension unpacking

---

_Pull request opened by @colin99d on 2023-01-01 19:27_

This is a part of #827. I tried using `libcst_native` to create the new item, but the SetComp items uses curly brackets. Since this is a really simple fix I decided to use the replace method. However, if you want this changed let me know.

---

_Renamed from "Bytes literal" to "Implement list comprehension unpacking-to-tuple comprehension" by @charliermarsh on 2023-01-01 21:40_

---

_Renamed from "Implement list comprehension unpacking-to-tuple comprehension" to "Implement list-to-tuple comprehension unpacking" by @charliermarsh on 2023-01-01 21:40_

---

_Merged by @charliermarsh on 2023-01-01 21:53_

---

_Closed by @charliermarsh on 2023-01-01 21:53_

---

_Comment by @andersk on 2023-01-01 23:04_

FYI there’s no such thing as a “tuple comprehension”. `(fn(x) for x in items)` returns a generator object and is called a [generator expression](https://docs.python.org/3/reference/expressions.html#generator-expressions).

- #1540

---

_Comment by @charliermarsh on 2023-01-01 23:31_

Thanks, sorry, I'm tired!

---

_Comment by @colin99d on 2023-01-01 23:34_

Thanks for the catch!

---

_Branch deleted on 2023-01-01 23:34_

---
