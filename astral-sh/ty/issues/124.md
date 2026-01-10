```yaml
number: 124
title: support large unions of literals
type: issue
state: open
author: carljm
labels:
  - set-theoretic types
assignees: []
created_at: 2025-04-16T02:59:04Z
updated_at: 2025-05-11T07:38:35Z
url: https://github.com/astral-sh/ty/issues/124
synced_at: 2026-01-10T02:34:09Z
```

# support large unions of literals

---

_Issue opened by @carljm on 2025-04-16 02:59_

With https://github.com/astral-sh/ruff/pull/17419 we set a low limit on the size of unions-of-literals. We will probably want to increase this limit (maybe to somewhere in the 4-5000 range?), so we can support e.g. exhaustiveness checks on large code-generated enum types.

In order to do this without exposing ourselves to exponential slowdowns, we will need to expand the optimization in https://github.com/astral-sh/ruff/pull/17403 so that, instead of applying just to `UnionBuilder`, it is applied throughout our union and intersection representation. (That is, groups of same-kind literals are kept in a set and can be handled as a single block, instead of repetitively.)

---

_Renamed from "[red-knot] support large unions of literals" to "support large unions of literals" by @MichaReiser on 2025-05-07 15:25_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:38_

---
