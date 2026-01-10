```yaml
number: 16515
title: "[red-knot] De-duplicate symbol table query"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/deduplicate
created_at: 2025-03-05T07:32:50Z
updated_at: 2025-03-05T07:36:22Z
url: https://github.com/astral-sh/ruff/pull/16515
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] De-duplicate symbol table query

---

_Pull request opened by @dhruvmanila on 2025-03-05 07:32_

## Summary

This PR does a small refactor to avoid double `symbol_table(...).symbol(...)` call to check for `__slots__` and `TYPE_CHECKING`. It merges them into a single call.

I noticed this while looking at https://github.com/astral-sh/ruff/pull/16468.

---

_Label `internal` added by @dhruvmanila on 2025-03-05 07:32_

---

_Label `red-knot` added by @dhruvmanila on 2025-03-05 07:32_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-05 07:32_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-05 07:32_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-05 07:32_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-05 07:32_

---

_Renamed from "[red-knot] De-duplicate symbol lookup" to "[red-knot] De-duplicate symbol table query" by @dhruvmanila on 2025-03-05 07:33_

---

_Merged by @dhruvmanila on 2025-03-05 07:36_

---

_Closed by @dhruvmanila on 2025-03-05 07:36_

---

_Branch deleted on 2025-03-05 07:36_

---
