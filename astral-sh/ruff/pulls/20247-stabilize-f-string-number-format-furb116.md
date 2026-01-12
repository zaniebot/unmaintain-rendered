```yaml
number: 20247
title: "Stabilize `f-string-number-format` (`FURB116`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: brent/furb116
created_at: 2025-09-04T18:26:35Z
updated_at: 2025-09-05T14:25:57Z
url: https://github.com/astral-sh/ruff/pull/20247
synced_at: 2026-01-12T15:56:57Z
```

# Stabilize `f-string-number-format` (`FURB116`)

---

_@ntBre_

Tests and docs look good


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 18:26_

---

_Label `rule` added by @ntBre on 2025-09-04 18:26_

---

_Comment by @github-actions[bot] on 2025-09-04 18:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-09-04 19:54_

---

_Marked ready for review by @ntBre on 2025-09-04 19:54_

---

_@amyreese approved on 2025-09-04 23:33_

---

_@AlexWaygood approved on 2025-09-05 13:58_

Not sure I agree with the docs here that using an f-string is more readable than using `oct()` 😄

But the `FURB` rules are often pretty opinionated, and it's fairly easy to disable the rule if you disagree, I guess

---

_Comment by @ntBre on 2025-09-05 14:01_

Ah really?? I guess neither of them is really that readable, but I thought the f-string was slightly better :laughing: We can hold off if you feel strongly, I'm happy either way.

---

_Comment by @AlexWaygood on 2025-09-05 14:12_

I don't have a strong opinion. I don't think either `bin()` or `oct()` is _commonly_ used, so consistency is probably the most important thing 😄

---

_Merged by @ntBre on 2025-09-05 14:25_

---

_Closed by @ntBre on 2025-09-05 14:25_

---

_Branch deleted on 2025-09-05 14:25_

---
