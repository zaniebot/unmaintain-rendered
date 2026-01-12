```yaml
number: 18965
title: "[ty] Add regression test for leading tab mis-alignment in diagnostic rendering"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/diagnostic-render-tab-alignment-off
created_at: 2025-06-26T16:24:01Z
updated_at: 2025-06-26T16:36:09Z
url: https://github.com/astral-sh/ruff/pull/18965
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Add regression test for leading tab mis-alignment in diagnostic rendering

---

_@BurntSushi_

It turns out that astral-sh/ty#18692 also fixed astral-sh/ty#203. This
PR adds a regression test for it. (Locally, I "unfixed" the bug and
confirmed that this is actually a regression test.)

Fixes astral-sh/ty#203


---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-26 16:24_

---

_Label `bug` added by @BurntSushi on 2025-06-26 16:24_

---

_Label `ty` added by @BurntSushi on 2025-06-26 16:24_

---

_Label `diagnostics` added by @BurntSushi on 2025-06-26 16:24_

---

_@AlexWaygood approved on 2025-06-26 16:25_

nice!

---

_Merged by @BurntSushi on 2025-06-26 16:27_

---

_Closed by @BurntSushi on 2025-06-26 16:27_

---

_Branch deleted on 2025-06-26 16:27_

---

_Comment by @github-actions[bot] on 2025-06-26 16:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
