```yaml
number: 5929
title: "Formatter: `TypeAlias`"
type: issue
state: closed
author: konstin
labels:
  - formatter
  - python312
assignees: []
created_at: 2023-07-20T19:42:34Z
updated_at: 2023-08-24T20:02:27Z
url: https://github.com/astral-sh/ruff/issues/5929
synced_at: 2026-01-12T15:54:45Z
```

# Formatter: `TypeAlias`

---

_@konstin_

See [PEP 695](https://peps.python.org/pep-0695/) for details on this python 3.12 feature. Not to be confused with `typing.TypeAlias` of python 3.10. The goal is to format statements in the form:

```python
# TypeAlias
type ListOrSet[T] = list[T] | set[T]
```

---

_Assigned to @zanieb by @konstin on 2023-07-20 19:42_

---

_Label `formatter` added by @konstin on 2023-07-20 19:48_

---

_Comment by @zanieb on 2023-07-20 20:16_

Technically probably best to implement formatting for type parameters on functions and classes first as the implementation for that will need to be reused here.

---

_Renamed from "Formatter: `TypeAlias`" to "Formatter: `TypeAlias` and `TypeParameters`" by @MichaReiser on 2023-07-20 21:21_

---

_Comment by @MichaReiser on 2023-07-20 21:22_

> Technically probably best to implement formatting for type parameters on functions and classes first as the implementation for that will need to be reused here.

I included type parameters in the issue and its description

---

_Comment by @konstin on 2023-07-20 21:26_

i meanwhile had made a new issue: https://github.com/astral-sh/ruff/issues/5931

---

_Comment by @MichaReiser on 2023-07-20 21:28_

> i meanwhile had made a new issue: https://github.com/astral-sh/ruff/issues/5931

Oh I'm sorry. I defer the decision on whether to revert my changes or close this issue to you. 

---

_Renamed from "Formatter: `TypeAlias` and `TypeParameters`" to "Formatter: `TypeAlias`" by @konstin on 2023-07-21 15:58_

---

_Comment by @konstin on 2023-07-21 15:59_

i've reverted, i think they'll require two separate PRs

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 16:08_

---

_Closed by @zanieb on 2023-08-02 20:40_

---

_Label `python312` added by @zanieb on 2023-08-24 20:02_

---
