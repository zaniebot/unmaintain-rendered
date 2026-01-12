```yaml
number: 14703
title: "[red-knot] understand `type[A | B]` where A and B are both classes"
type: issue
state: closed
author: Glyphack
labels:
  - ty
assignees: []
created_at: 2024-12-01T20:32:31Z
updated_at: 2024-12-07T17:34:51Z
url: https://github.com/astral-sh/ruff/issues/14703
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] understand `type[A | B]` where A and B are both classes

---

_@Glyphack_

(part of https://github.com/astral-sh/ruff/issues/14172)
`type[A | B]` Is equivalent to an enum of `type[A]` | `type[B]`.

There are not a lot of edge cases looking at the tests here: https://github.com/python/typing/blob/main/conformance/tests/specialtypes_type.py#L50

---

_Label `red-knot` added by @AlexWaygood on 2024-12-01 20:37_

---

_Assigned to @Glyphack by @AlexWaygood on 2024-12-01 20:37_

---

_Comment by @sbrugman on 2024-12-01 22:46_

Just a heads-up for future reference: red-knot should produce a violation when the `type[A, B]` syntax is used.

This came up in [this PR](https://github.com/astral-sh/ruff/pull/14660)

---

_Closed by @carljm on 2024-12-07 17:34_

---
