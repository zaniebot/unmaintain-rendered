```yaml
number: 391
title: improve diagnostic context for unsupported-operator
type: issue
state: open
author: carljm
labels:
  - diagnostics
assignees: []
created_at: 2025-05-14T18:06:21Z
updated_at: 2025-11-14T14:47:36Z
url: https://github.com/astral-sh/ty/issues/391
synced_at: 2026-01-12T15:54:23Z
```

# improve diagnostic context for unsupported-operator

---

_@carljm_

When we get `unsupported-operator` errors from custom classes with custom (but somehow wrong) dunder methods defined, we currently don't offer any context on what actually went wrong in trying to make the implicit call to implement the operator.

https://github.com/astral-sh/ty/issues/315 shows one case where this made it difficult for the user to understand the nature of the problem.

---

_Label `diagnostics` added by @carljm on 2025-05-14 18:06_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:47_

---
