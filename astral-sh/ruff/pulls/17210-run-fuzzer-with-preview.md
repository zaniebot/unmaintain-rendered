```yaml
number: 17210
title: "Run fuzzer with `--preview`"
type: pull_request
state: merged
author: ntBre
labels:
  - fuzzer
assignees: []
merged: true
base: main
head: brent/fuzz-preview
created_at: 2025-04-04T17:36:49Z
updated_at: 2025-04-04T19:14:00Z
url: https://github.com/astral-sh/ruff/pull/17210
synced_at: 2026-01-10T19:40:37Z
```

# Run fuzzer with `--preview`

---

_Pull request opened by @ntBre on 2025-04-04 17:36_

Summary
--

Updates `fuzz.py` to run with `--preview`, which should allow it to catch semantic syntax errors.

Test Plan
--

@AlexWaygood and I temporarily made any named expression a semantic syntax error and checked that this led to fuzzing errors. We also tested that reverting the `--preview` addition did not show any errors.

We also ran the fuzzer on 500 seeds on `main` but didn't find any issues, (un)fortunately.


---

_Label `fuzzer` added by @ntBre on 2025-04-04 17:36_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-04 17:36_

---

_@MichaReiser approved on 2025-04-04 17:39_

---

_Comment by @github-actions[bot] on 2025-04-04 17:46_

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

_@AlexWaygood approved on 2025-04-04 17:48_

---

_Merged by @ntBre on 2025-04-04 19:13_

---

_Closed by @ntBre on 2025-04-04 19:13_

---

_Branch deleted on 2025-04-04 19:14_

---
