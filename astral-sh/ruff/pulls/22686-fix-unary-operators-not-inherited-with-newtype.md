```yaml
number: 22686
title: Fix unary operators not inherited with NewType
type: pull_request
state: closed
author: kirankotari
labels: []
assignees: []
base: main
head: fix/newtype-unary-operators
created_at: 2026-01-18T19:25:15Z
updated_at: 2026-01-18T19:39:30Z
url: https://github.com/astral-sh/ruff/pull/22686
synced_at: 2026-01-18T20:18:40Z
```

# Fix unary operators not inherited with NewType

---

_@kirankotari_

Support unary operators (, , ) for NewType instances by following
the same pattern as binary operators: first try calling the dunder method,
and fall back to the concrete base type if that fails.

This is needed for NewTypes of float and complex where the base type is a union.

Fixes astral-sh/ty#2499


---

_Review requested from @carljm by @kirankotari on 2026-01-18 19:25_

---

_Review requested from @AlexWaygood by @kirankotari on 2026-01-18 19:25_

---

_Review requested from @sharkdp by @kirankotari on 2026-01-18 19:25_

---

_Review requested from @dcreager by @kirankotari on 2026-01-18 19:25_

---

_Comment by @AlexWaygood on 2026-01-18 19:39_

Thanks for the PR. Unfortunately, there is already an open PR that is linked to the issue you're trying to fix (https://github.com/astral-sh/ruff/pull/22605)

---

_Closed by @AlexWaygood on 2026-01-18 19:39_

---
