---
number: 14988
title: "[red-knot] fully consolidate display of heterogenous unions of literals"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-12-15T18:02:36Z
updated_at: 2025-01-08T00:58:40Z
url: https://github.com/astral-sh/ruff/issues/14988
synced_at: 2026-01-10T01:22:55Z
---

# [red-knot] fully consolidate display of heterogenous unions of literals

---

_Issue opened by @carljm on 2024-12-15 18:02_

We currently display `Literal[1, 2] | Literal["foo"]`, but the spec (and other type checkers) prefer `Literal[1, 2, "foo"]`. We allow the latter in an annotation, but we don't display it that way.

We should probably switch to displaying this like the spec and other type checkers do.

I would leave class and function literals out of this change, since they aren't part of the spec and we may want to display them entirely differently.

---

_Label `red-knot` added by @carljm on 2024-12-15 18:02_

---

_Referenced in [astral-sh/ruff#14981](../../astral-sh/ruff/issues/14981.md) on 2024-12-15 18:02_

---

_Label `help wanted` added by @carljm on 2024-12-15 18:02_

---

_Referenced in [astral-sh/ruff#14993](../../astral-sh/ruff/pulls/14993.md) on 2024-12-15 20:32_

---

_Closed by @carljm on 2025-01-08 00:58_

---
