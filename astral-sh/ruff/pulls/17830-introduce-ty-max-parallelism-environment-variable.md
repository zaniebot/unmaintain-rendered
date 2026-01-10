```yaml
number: 17830
title: "Introduce `TY_MAX_PARALLELISM` environment variable"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/max-parallelism
created_at: 2025-05-04T13:25:04Z
updated_at: 2025-05-04T14:27:17Z
url: https://github.com/astral-sh/ruff/pull/17830
synced_at: 2026-01-10T18:57:03Z
```

# Introduce `TY_MAX_PARALLELISM` environment variable

---

_Pull request opened by @MichaReiser on 2025-05-04 13:25_

## Summary

This PR introduces a new `max_parallelism` helper in `ruff_db` that returns the maximum number of threads ty should use when processing tasks in parallel (checking files, indexing a directory). 

The helper defaults to `std::thread::available_parallelism` but the value can be overridden using the `TY_MAX_PARALLELISM` or `RAYON_NUM_THREADS` environment variables (where `TY_MAX_PARALLELISM takes precedence over `RAYON_NUM_THREADS`). 

The difference to rayons default handling is that ty also respects this upper bound for operations that don't run on a rayon thread pool, e.g. walking a directory tree. 


## Test plan

I verified that running ty with `TY_MAX_PARALLELISM=1 ../ruff/target/debug/ty check -vvv` doesn't result in any log interleaving (only a single thread) whreas running `../ruff/target/debug/ty check -vvv` does

---

_Review requested from @carljm by @MichaReiser on 2025-05-04 13:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-04 13:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-04 13:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-04 13:25_

---

_Label `red-knot` added by @MichaReiser on 2025-05-04 13:25_

---

_Comment by @github-actions[bot] on 2025-05-04 13:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-04 14:23_

---

_Merged by @MichaReiser on 2025-05-04 14:27_

---

_Closed by @MichaReiser on 2025-05-04 14:27_

---

_Branch deleted on 2025-05-04 14:27_

---
