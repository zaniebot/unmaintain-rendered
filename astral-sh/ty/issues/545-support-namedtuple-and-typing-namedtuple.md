```yaml
number: 545
title: support namedtuple and typing.NamedTuple
type: issue
state: closed
author: carljm
labels:
  - feature
  - typing semantics
  - namedtuples
assignees: []
created_at: 2025-05-30T01:50:56Z
updated_at: 2025-11-29T18:36:52Z
url: https://github.com/astral-sh/ty/issues/545
synced_at: 2026-01-12T15:54:23Z
```

# support namedtuple and typing.NamedTuple

---

_@carljm_

_No description provided._

---

_Label `feature` added by @carljm on 2025-05-30 01:50_

---

_Label `typing semantics` added by @carljm on 2025-05-30 01:50_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-30 15:56_

---

_Comment by @sharkdp on 2025-06-03 07:41_

Just a reference to the existing `typing.NamedTuple` preliminary support: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/named_tuple.md

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:47_

---

_Comment by @y2kbugger on 2025-08-14 04:05_

https://play.ty.dev/c485b33f-b765-4908-8a8b-1b0214ff5239

---

_Comment by @carljm on 2025-08-15 15:07_

Need to add a check to disallow multiple inheritance, and add support for functional syntax.

---

_Comment by @AlexWaygood on 2025-08-15 15:13_

> Need to add a check to disallow multiple inheritance, and add support for functional syntax.

(the second of which is probably not a beta priority)

---

_Comment by @carljm on 2025-08-15 16:28_

> https://play.ty.dev/c485b33f-b765-4908-8a8b-1b0214ff5239

Thanks for the report! We're discussing/fixing this over in https://github.com/astral-sh/ruff/pull/19915

---

_Closed by @AlexWaygood on 2025-08-19 08:56_

---

_Comment by @AlexWaygood on 2025-08-19 11:35_

Opened a followup issue here for the last remaining big task for full namedtuple support, which is support for the function-call syntax for defining namedtuples: https://github.com/astral-sh/ty/issues/1049

---

_Label `namedtuples` added by @AlexWaygood on 2025-11-29 18:36_

---
