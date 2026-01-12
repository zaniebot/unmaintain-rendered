```yaml
number: 22127
title: "[ty] Rename benchmarks, update iteration counts"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
draft: true
base: micha/better-balanced-walltime-shardiing
head: micha/rename-benchmarks
created_at: 2025-12-21T12:32:32Z
updated_at: 2025-12-21T20:15:38Z
url: https://github.com/astral-sh/ruff/pull/22127
synced_at: 2026-01-12T15:57:42Z
```

# [ty] Rename benchmarks, update iteration counts

---

_@MichaReiser_

## Summary

Remove the `small`, `medium` and `large` groups because projects that used to be very fast
to type check now take longer and would have to be moved into another group so that we can update the iteration counts.

This PR also updates the iteration counts for `colour_science` and `pandas` (from 3 to 2, equal to moving them to large) and
pydantic (from 1 to 3, moving it from large to medium).

The downside of this is that we lose our historical data but this is a better long-term setup.


| Benchmark | Time per iter | Iterations | Total |
|-----------|---------------|------------|-------|
| colour_science | 1.5 min | 2 | ~3 min |
| pandas | 1 min | 2 | ~2 min |
| sympy | 52s | 2 | ~1.7 min |
| static_frame | 20s | 3 | ~1 min |
| pydantic | 11s | 3 | ~33s |
| freqtrade | 8s | 6 | ~48s |
| altair | 5s | 6 | ~30s |
| tanjun | 2.5s | 6 | ~15s |
| multithreaded | 1.4s | 24 | ~34s |


## Test Plan

CI


---

_Label `testing` added by @MichaReiser on 2025-12-21 12:32_

---

_Label `ty` added by @MichaReiser on 2025-12-21 12:32_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 12:42_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Merged by @MichaReiser on 2025-12-21 20:15_

---

_Closed by @MichaReiser on 2025-12-21 20:15_

---

_Branch deleted on 2025-12-21 20:15_

---
