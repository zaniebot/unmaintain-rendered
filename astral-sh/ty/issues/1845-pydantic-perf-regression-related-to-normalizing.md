```yaml
number: 1845
title: "Pydantic perf regression related to normalizing `TypedDict`s"
type: issue
state: closed
author: oconnor663
labels:
  - performance
assignees: []
created_at: 2025-12-10T19:42:43Z
updated_at: 2025-12-10T20:33:41Z
url: https://github.com/astral-sh/ty/issues/1845
synced_at: 2026-01-10T01:56:41Z
```

# Pydantic perf regression related to normalizing `TypedDict`s

---

_Issue opened by @oconnor663 on 2025-12-10 19:42_

This was a known regression that came with https://github.com/astral-sh/ruff/pull/21784.

The cause is calls to `normalized` that happen inside of `FunctionType::has_relation_to_impl`, [as described here](https://github.com/astral-sh/ruff/pull/21784#issuecomment-3633949961). Those calls are themselves a performance optimization, and deleting them fixes this regression, [as described here](https://github.com/astral-sh/ruff/pull/21784#issuecomment-3638357067). We'll want to come back to this after `TypedDict` "tagged union narrowing" is in (https://github.com/astral-sh/ty/issues/1479#issuecomment-3513914142), to see how much of the regression remains.

---

_Label `performance` added by @oconnor663 on 2025-12-10 19:42_

---

_Comment by @oconnor663 on 2025-12-10 20:33_

Oh never mind, we're actually going to land that deletion immediately. Tally ho!

---

_Closed by @oconnor663 on 2025-12-10 20:33_

---
