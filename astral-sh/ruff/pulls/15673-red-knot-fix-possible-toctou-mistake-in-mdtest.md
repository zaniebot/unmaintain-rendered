```yaml
number: 15673
title: "[red-knot] Fix possible TOCTOU mistake in mdtest runner"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-runner-catch-cargo-test-error
created_at: 2025-01-22T15:19:42Z
updated_at: 2025-01-22T15:26:05Z
url: https://github.com/astral-sh/ruff/pull/15673
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Fix possible TOCTOU mistake in mdtest runner

---

_Pull request opened by @sharkdp on 2025-01-22 15:19_

## Summary

*Somehow*, I managed to crash the `mdtest` runner today. I struggled to reproduce this again to see if it's actually fixed (even with an artificial `sleep` between the two `cargo test` invocations), but the original backtrace clearly showed that this is where the problem originated from. And it seems like a clear TOCTOU problem.

## Test Plan

:man_shrugging: 

---

_Label `red-knot` added by @sharkdp on 2025-01-22 15:19_

---

_Review requested from @carljm by @sharkdp on 2025-01-22 15:19_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-22 15:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-22 15:19_

---

_Merged by @sharkdp on 2025-01-22 15:24_

---

_Closed by @sharkdp on 2025-01-22 15:24_

---

_Branch deleted on 2025-01-22 15:24_

---

_Comment by @github-actions[bot] on 2025-01-22 15:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
