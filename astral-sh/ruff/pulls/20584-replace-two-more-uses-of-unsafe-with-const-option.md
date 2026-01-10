```yaml
number: 20584
title: "Replace two more uses of unsafe with const `Option::unwrap`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/safe-const-unwrap
created_at: 2025-09-25T19:08:05Z
updated_at: 2025-09-25T19:35:15Z
url: https://github.com/astral-sh/ruff/pull/20584
synced_at: 2026-01-10T17:40:28Z
```

# Replace two more uses of unsafe with const `Option::unwrap`

---

_Pull request opened by @ntBre on 2025-09-25 19:08_

I guess I missed these in #20007, but I found them today while grepping for something else. `Option::unwrap` has been const since 1.83, so we can use it here and avoid some unsafe code.


---

_Label `internal` added by @ntBre on 2025-09-25 19:08_

---

_Comment by @github-actions[bot] on 2025-09-25 19:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-09-25 19:17_

---

_Review requested from @MichaReiser by @ntBre on 2025-09-25 19:17_

---

_@dylwil3 approved on 2025-09-25 19:32_

---

_@MichaReiser approved on 2025-09-25 19:34_

---

_Merged by @ntBre on 2025-09-25 19:35_

---

_Closed by @ntBre on 2025-09-25 19:35_

---

_Branch deleted on 2025-09-25 19:35_

---
