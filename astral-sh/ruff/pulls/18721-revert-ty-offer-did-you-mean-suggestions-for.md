```yaml
number: 18721
title: " Revert \"[ty] Offer \"Did you mean...?\" suggestions for unresolved `from` imports and unresolved attributes (#18705)\""
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/debug-primer
created_at: 2025-06-17T13:20:05Z
updated_at: 2025-06-17T14:48:11Z
url: https://github.com/astral-sh/ruff/pull/18721
synced_at: 2026-01-10T18:39:08Z
```

#  Revert "[ty] Offer "Did you mean...?" suggestions for unresolved `from` imports and unresolved attributes (#18705)"

---

_Pull request opened by @AlexWaygood on 2025-06-17 13:20_

## Summary

The change meant that we never finished type-checking `dd-trace-py`, meaning mypy_primer runs never finished. Apologies -- I should have checked my analysis in https://github.com/astral-sh/ruff/pull/18705#issuecomment-2978338496 more thoroughly before merging #18705.

## Test Plan

Mypy primer runs are successfully finishing on https://github.com/astral-sh/ruff/pull/18722, which is based on top of this branch. (I wouldn't expect primer runs to finish on this PR, because `ty_old` is the `main` branch here, which is still a version of ty that will never finish checking dd-trace-py.)


---

_Renamed from "have i broken mypy_primer" to " Revert "[ty] Offer "Did you mean...?" suggestions for unresolved `from` imports and unresolved attributes (#18705)"" by @AlexWaygood on 2025-06-17 14:40_

---

_Label `ty` added by @AlexWaygood on 2025-06-17 14:43_

---

_Marked ready for review by @AlexWaygood on 2025-06-17 14:44_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-17 14:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-17 14:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-17 14:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-17 14:44_

---

_Comment by @AlexWaygood on 2025-06-17 14:45_

(Note: @sharkdp thinks the bug is in `all_members`, which means there's probably a way of triggering a hang from exactly the wrong autocomplete request. This revert doesn't fix the underlying bug, but it means mypy_primer runs start finishing again, which is important!)

---

_@sharkdp approved on 2025-06-17 14:47_

---

_Merged by @AlexWaygood on 2025-06-17 14:48_

---

_Closed by @AlexWaygood on 2025-06-17 14:48_

---

_Branch deleted on 2025-06-17 14:48_

---
