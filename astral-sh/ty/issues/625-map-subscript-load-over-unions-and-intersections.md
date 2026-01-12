```yaml
number: 625
title: map subscript load over unions and intersections
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - typing semantics
assignees: []
created_at: 2025-06-10T15:41:39Z
updated_at: 2025-06-23T16:38:03Z
url: https://github.com/astral-sh/ty/issues/625
synced_at: 2026-01-12T15:54:23Z
```

# map subscript load over unions and intersections

---

_@carljm_

> An example I got from reviewing https://github.com/astral-sh/ruff/pull/17643, which may or may not be related to this issue. I find it strange that we infer `Unknown` here, and not some `@Todo` type?
> 
> 
> ```py
> from typing import reveal_type
> 
> 
> def _(union_of_tuples: tuple[int] | tuple[None]):
>     reveal_type(union_of_tuples[0])  # Unknown
> ``` 

 _Originally posted by @sharkdp in [#493](https://github.com/astral-sh/ty/issues/493#issuecomment-2958399376)_

It looks to me like the issue here is that `TypeInferenceBuilder::infer_subscript_expression_types` has no case for handling unions or intersections, by mapping the subscript operation over the element types and unioning/intersecting the results.

---

_Label `help wanted` added by @carljm on 2025-06-10 15:41_

---

_Label `typing semantics` added by @carljm on 2025-06-10 15:41_

---

_Comment by @suneettipirneni on 2025-06-14 23:13_

If possible, I would like to take a crack at this.

---

_Assigned to @suneettipirneni by @carljm on 2025-06-17 02:10_

---

_Closed by @carljm on 2025-06-23 16:38_

---
