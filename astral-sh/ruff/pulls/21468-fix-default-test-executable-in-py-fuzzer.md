```yaml
number: 21468
title: Fix default test executable in py-fuzzer
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/fuzz
created_at: 2025-11-15T02:55:59Z
updated_at: 2025-11-15T18:12:12Z
url: https://github.com/astral-sh/ruff/pull/21468
synced_at: 2026-01-10T16:53:56Z
```

# Fix default test executable in py-fuzzer

---

_Pull request opened by @ntBre on 2025-11-15 02:55_

Summary
--

I was firing up the fuzzer tonight and hit an assertion error here. We now build with the `profiling` profile, so we need to use that executable too.

This hasn't affected CI because we always set the `--test-executable`.

Test Plan
--

Ran the script again with the same arguments on this branch

---

_Review requested from @AlexWaygood by @ntBre on 2025-11-15 02:56_

---

_Label `internal` added by @ntBre on 2025-11-15 02:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 03:04_


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

_@MichaReiser approved on 2025-11-15 08:59_

---

_@AlexWaygood approved on 2025-11-15 10:46_

Thanks!

---

_Merged by @ntBre on 2025-11-15 18:12_

---

_Closed by @ntBre on 2025-11-15 18:12_

---

_Branch deleted on 2025-11-15 18:12_

---
