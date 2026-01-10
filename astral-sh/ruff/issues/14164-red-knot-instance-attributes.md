```yaml
number: 14164
title: "[red-knot] instance attributes"
type: issue
state: closed
author: carljm
labels:
  - tracking
  - ty
assignees: []
created_at: 2024-11-07T15:33:00Z
updated_at: 2025-07-07T13:04:30Z
url: https://github.com/astral-sh/ruff/issues/14164
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] instance attributes

---

_Issue opened by @carljm on 2024-11-07 15:33_

- [x] Write test suite with desired behavior (resolved by https://github.com/astral-sh/ruff/pull/15474 and https://github.com/astral-sh/ruff/pull/15474)
- [x] Support pure instance variables, declared in class body (resolved by https://github.com/astral-sh/ruff/pull/15515)
- [x] Add support for `typing.ClassVar` (resolved by https://github.com/astral-sh/ruff/pull/15550)
- [x] Raise diagnostics for invalid assignments to attributes (see https://github.com/astral-sh/ruff/issues/15456; resolved by https://github.com/astral-sh/ruff/pull/15613, https://github.com/astral-sh/ruff/pull/15684 and https://github.com/astral-sh/ruff/pull/15668)
- [x] Infer `Unknown | T_inferred` for undeclared class variables (resolved by https://github.com/astral-sh/ruff/pull/15674)
- [x] Support implicit instance attributes, which are not defined in the class body, only in `__init__` or another method (resolved by https://github.com/astral-sh/ruff/pull/15811)
- [x] Support for possibly-unbound instance attributes (resolved by https://github.com/astral-sh/ruff/pull/16363)
- [x] astral-sh/ruff#15962
- [x] astral-sh/ruff#15963
- [x] astral-sh/ruff#15960
- [x] astral-sh/ty#205
- [x] astral-sh/ty#207
- [ ] Handle cycles in type inference once we understand the type of `self`
- [x] astral-sh/ruff#15966
- [ ] astral-sh/ty#206
- [x] astral-sh/ruff#15967
- [x] Implement method call checking (resolved by https://github.com/astral-sh/ruff/pull/16121)
- [x] astral-sh/ty#211

See also: Markdown test suite with some open TODOs: https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/attributes.md

---

_Label `red-knot` added by @carljm on 2024-11-07 15:33_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:33_

---

_Assigned to @sharkdp by @carljm on 2024-11-15 15:49_

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:43_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:43_

---

_Comment by @sharkdp on 2025-01-27 14:45_

Modified the description to reflect the current progress

---

_Removed from milestone `Red Knot Q1 2025` by @carljm on 2025-03-26 12:04_

---

_Label `tracking` added by @carljm on 2025-03-26 12:05_

---

_Added to milestone `Red Knot GA` by @carljm on 2025-03-27 18:43_

---

_Unassigned @sharkdp by @carljm on 2025-03-27 18:44_

---

_Comment by @sharkdp on 2025-04-02 14:19_

I think we can actually close this tracking ticket. All remaining sub-items have tickets, and most of them are not really related to *instance* attributes anyway.

---

_Closed by @sharkdp on 2025-04-02 14:19_

---
