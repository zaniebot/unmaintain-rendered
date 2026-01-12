```yaml
number: 21568
title: "[ty] Remove brittle constraint set reveal tests"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/remove-reveals
created_at: 2025-11-21T18:27:38Z
updated_at: 2025-11-21T18:57:57Z
url: https://github.com/astral-sh/ruff/pull/21568
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Remove brittle constraint set reveal tests

---

_@dcreager_

These were added to try to make it clearer that assignability checks will eventually return more detailed answers than true or false. However, the constraint set display rendering is still more brittle than I'd like it to be, and it's more trouble than it's worth to keep them updated with semantically identically but textually different edits. The `static_assert`s are sufficient to check correctness, and we can always add `reveal_type` when needed for further debugging.

---

_Label `internal` added by @dcreager on 2025-11-21 18:27_

---

_Review requested from @carljm by @dcreager on 2025-11-21 18:27_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-21 18:27_

---

_Review requested from @sharkdp by @dcreager on 2025-11-21 18:27_

---

_Label `ty` added by @dcreager on 2025-11-21 18:27_

---

_Comment by @dcreager on 2025-11-21 18:28_

This is hopefully uncontroversial, and doesn't touch any code, so I plan to merge it without review once CI goes green

---

_Merged by @dcreager on 2025-11-21 18:57_

---

_Closed by @dcreager on 2025-11-21 18:57_

---

_Branch deleted on 2025-11-21 18:57_

---
