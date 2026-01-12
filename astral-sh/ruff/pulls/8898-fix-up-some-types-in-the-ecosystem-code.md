```yaml
number: 8898
title: Fix up some types in the ecosystem code
type: pull_request
state: merged
author: diceroll123
labels:
  - internal
assignees: []
merged: true
base: main
head: ecosystem-type-updates
created_at: 2023-11-29T03:29:27Z
updated_at: 2023-11-30T22:02:25Z
url: https://github.com/astral-sh/ruff/pull/8898
synced_at: 2026-01-10T23:40:55Z
```

# Fix up some types in the ecosystem code

---

_Pull request opened by @diceroll123 on 2023-11-29 03:29_

## Summary

Fixes up the type annotations to make type analyzers a little happier 😄 

## Test Plan

N/A

---

_Comment by @github-actions[bot] on 2023-11-29 03:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb reviewed on 2023-11-29 03:49_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/main.py`:56 on 2023-11-29 03:49_

Eek. Is this the type annotation for `asyncio.gather`?

---

_@zanieb reviewed on 2023-11-29 03:50_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/main.py`:78 on 2023-11-29 03:50_

I don't want to swallow base exceptions, we should technically re-raise those I guess?

---

_@diceroll123 reviewed on 2023-11-29 04:07_

---

_Review comment by @diceroll123 on `python/ruff-ecosystem/ruff_ecosystem/main.py`:56 on 2023-11-29 04:07_

Yep, I think it's due to [asyncio.CancelledError being a subclass of BaseException, it previously was Exception though](https://docs.python.org/3/library/asyncio-exceptions.html#asyncio.CancelledError).

---

_Marked ready for review by @diceroll123 on 2023-11-29 04:07_

---

_@zanieb approved on 2023-11-30 22:02_

---

_Merged by @zanieb on 2023-11-30 22:02_

---

_Closed by @zanieb on 2023-11-30 22:02_

---

_Label `internal` added by @zanieb on 2023-11-30 22:02_

---
