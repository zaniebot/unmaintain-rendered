```yaml
number: 14367
title: Split the corpus tests into smaller tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/split-corpus-tests
created_at: 2024-11-15T18:38:32Z
updated_at: 2024-11-16T19:29:24Z
url: https://github.com/astral-sh/ruff/pull/14367
synced_at: 2026-01-10T20:50:57Z
```

# Split the corpus tests into smaller tests

---

_Pull request opened by @MichaReiser on 2024-11-15 18:38_

## Summary

This PR splits the corpus tests into smaller chunks because running all of them takes 8s on my windows machine and it's by far the longest test in `red_knot_workspace`. 

Splitting the tests has the advantage that they run in parallel. This PR brings down the wall time from 8s to 4s. 

This PR also limits the glob for the linter tests because it's common to clone cpython into the `ruff_linter/resources/test` folder for benchmarks (because that's what's written in the contributing guides)

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-15 18:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-15 18:38_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-15 18:38_

---

_Label `testing` added by @MichaReiser on 2024-11-15 18:38_

---

_Label `red-knot` added by @MichaReiser on 2024-11-15 18:38_

---

_Renamed from "Split the corpus tests" to "Split the corpus tests into smaller tests" by @MichaReiser on 2024-11-15 18:40_

---

_Comment by @github-actions[bot] on 2024-11-15 18:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-11-15 23:47_

---

_@sharkdp approved on 2024-11-16 19:26_

Thank you!

---

_Merged by @sharkdp on 2024-11-16 19:29_

---

_Closed by @sharkdp on 2024-11-16 19:29_

---

_Branch deleted on 2024-11-16 19:29_

---
