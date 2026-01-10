---
number: 10670
title: "Feature request: rule that completely disallows typing.cast"
type: issue
state: open
author: wyattscarpenter
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-03-30T16:00:11Z
updated_at: 2024-03-30T17:32:13Z
url: https://github.com/astral-sh/ruff/issues/10670
synced_at: 2026-01-10T01:22:50Z
---

# Feature request: rule that completely disallows typing.cast

---

_Issue opened by @wyattscarpenter on 2024-03-30 16:00_

[cast()](https://docs.python.org/3/library/typing.html#typing.cast) gets around static type safety without ensuring dynamic type correctness. Sometimes you have to use it—you know how it is—but it's not ideal. (For instance, [mypy documentation](https://mypy.readthedocs.io/en/stable/type_narrowing.html#casts) seems to imply the main use of cast is when the typechecker or type annotations aren't quite good enough.) That means, in my opinion, cast is a great candidate to have a rule disallowing its use, to ensure code quality in codebases where it is not needed, even if that rule is ultimately optional.

---

_Label `rule` added by @AlexWaygood on 2024-03-30 16:08_

---

_Label `needs-decision` added by @AlexWaygood on 2024-03-30 16:08_

---

_Comment by @AlexWaygood on 2024-03-30 17:32_

Cross-referencing the similar issues over at mypy:
- https://github.com/python/mypy/issues/17077
- https://github.com/python/mypy/issues/5756

---
