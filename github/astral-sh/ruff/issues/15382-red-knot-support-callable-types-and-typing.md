---
number: 15382
title: "[red-knot] Support callable types and `typing.Callable`"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-09T18:54:44Z
updated_at: 2025-04-01T19:38:38Z
url: https://github.com/astral-sh/ruff/issues/15382
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Support callable types and `typing.Callable`

---

_Issue opened by @carljm on 2025-01-09 18:54_

Our "callable type" should be able to represent a callable of arbitrary signature; `typing.Callable` can only spell a limited subset of those types.

This also includes implementing subtyping/assignability/equivalence for this type.

---

- [x] Add a new `CallableType` variant (https://github.com/astral-sh/ruff/pull/16493)
- [x] Use the new variant for `typing.Callable` (https://github.com/astral-sh/ruff/pull/16493)
- [x] Use the new variant for `lambda` expressions (https://github.com/astral-sh/ruff/pull/16547)
	- [x] Infer the return type of `lambda` expressions (https://github.com/astral-sh/ruff/pull/16695)
- [x] Implement support for calling a `typing.Callable` i.e., `call` method (https://github.com/astral-sh/ruff/pull/16888)
- [x] Implement various relationship methods for the new variant
  - [x] `is_assignable_to` (https://github.com/astral-sh/ruff/pull/16845)
  - [x] `is_gradual_equivalent_to` (https://github.com/astral-sh/ruff/pull/16634)
  - [x] `is_subtype_of` (https://github.com/astral-sh/ruff/pull/16804)
  - [x] `is_equivalent_to` (https://github.com/astral-sh/ruff/pull/16698)
  - [x] `is_disjoint_form` (#17094)
- [x] Other methods
  - [x] `is_fully_static` (https://github.com/astral-sh/ruff/pull/16633)
  - [x] `is_single_valued` (https://github.com/astral-sh/ruff/pull/16493)
  - [x] `is_singleton` (https://github.com/astral-sh/ruff/pull/16493)
  - [x] `member_lookup_with_policy` (https://github.com/astral-sh/ruff/pull/16618)
  - [x] `static_member` (https://github.com/astral-sh/ruff/pull/16618)
  - [x] `to_meta_type` (https://github.com/astral-sh/ruff/pull/16618)
- [x] Add property tests for relation methods between callable types (#17006)

---

_Label `red-knot` added by @carljm on 2025-01-09 18:54_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 18:54_

---

_Referenced in [astral-sh/ruff#14173](../../astral-sh/ruff/issues/14173.md) on 2025-01-10 15:12_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-01-10 15:13_

---

_Referenced in [astral-sh/ruff#16493](../../astral-sh/ruff/pulls/16493.md) on 2025-03-06 12:18_

---

_Referenced in [astral-sh/ruff#16547](../../astral-sh/ruff/pulls/16547.md) on 2025-03-11 05:38_

---

_Referenced in [astral-sh/ruff#16633](../../astral-sh/ruff/pulls/16633.md) on 2025-03-11 16:17_

---

_Referenced in [astral-sh/ruff#16634](../../astral-sh/ruff/pulls/16634.md) on 2025-03-11 16:29_

---

_Referenced in [astral-sh/ruff#16695](../../astral-sh/ruff/pulls/16695.md) on 2025-03-14 03:46_

---

_Referenced in [astral-sh/ruff#16804](../../astral-sh/ruff/pulls/16804.md) on 2025-03-19 04:45_

---

_Referenced in [astral-sh/ruff#16845](../../astral-sh/ruff/pulls/16845.md) on 2025-03-19 13:23_

---

_Referenced in [astral-sh/ruff#16887](../../astral-sh/ruff/pulls/16887.md) on 2025-03-21 08:49_

---

_Referenced in [astral-sh/ruff#16888](../../astral-sh/ruff/pulls/16888.md) on 2025-03-21 09:09_

---

_Renamed from "[red-knot] support callable types and typing.Callable" to "[red-knot] Support callable types and `typing.Callable`" by @dhruvmanila on 2025-03-21 09:11_

---

_Referenced in [astral-sh/ruff#17006](../../astral-sh/ruff/pulls/17006.md) on 2025-03-27 02:13_

---

_Removed from milestone `Red Knot Q1 2025` by @carljm on 2025-03-27 18:33_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Referenced in [astral-sh/ruff#17094](../../astral-sh/ruff/pulls/17094.md) on 2025-03-31 14:05_

---

_Comment by @dhruvmanila on 2025-04-01 19:38_

Marking this as done! Next-up is overloads!

---

_Closed by @dhruvmanila on 2025-04-01 19:38_

---
