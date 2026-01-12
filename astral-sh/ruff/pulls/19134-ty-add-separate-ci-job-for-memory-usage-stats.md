```yaml
number: 19134
title: "[ty] Add separate CI job for memory usage stats"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: ibraheem/mypy-primer-determinism
created_at: 2025-07-03T23:44:39Z
updated_at: 2025-07-07T16:22:00Z
url: https://github.com/astral-sh/ruff/pull/19134
synced_at: 2026-01-12T15:56:32Z
```

# [ty] Add separate CI job for memory usage stats

---

_@ibraheemdev_

## Summary

As discussed in https://github.com/astral-sh/ruff/pull/19059.

---

_Label `ci` added by @ibraheemdev on 2025-07-03 23:44_

---

_Label `ty` added by @ibraheemdev on 2025-07-03 23:44_

---

_Review comment by @ibraheemdev on `crates/ty/src/lib.rs`:301 on 2025-07-03 23:45_

This is a hack for now, we should have a way to silence diagnostics completely (but keep the memory usage report).

---

_@ibraheemdev reviewed on 2025-07-03 23:45_

---

_@ibraheemdev reviewed on 2025-07-03 23:45_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/primer/memory.txt`:4 on 2025-07-03 23:45_

Somewhat arbitrarily chosen, open to suggestions.

---

_@ibraheemdev reviewed on 2025-07-03 23:47_

---

_Review comment by @ibraheemdev on `crates/ty_project/src/db.rs`:337 on 2025-07-03 23:47_

I lowered the threshold because single-threaded runs should be completely deterministic, and the jump between buckets is currently quite high for large numbers.

---

_Comment by @github-actions[bot] on 2025-07-03 23:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @ibraheemdev on 2025-07-04 00:15_

We could also just run the memory usage report in the same job? If we choose smaller projects that shouldn't add significant time, and we avoid having to use another depot runner and duplicate the setup work. We could also avoid using mypy-primer for that run and get exact numbers.

---

_Marked ready for review by @ibraheemdev on 2025-07-04 00:32_

---

_Review requested from @carljm by @ibraheemdev on 2025-07-04 00:32_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-07-04 00:32_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-07-04 00:32_

---

_Review requested from @dcreager by @ibraheemdev on 2025-07-04 00:32_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-07-04 00:32_

---

_Review comment by @AlexWaygood on `.github/workflows/mypy_primer.yaml`:102 on 2025-07-04 10:37_

could this be extracted as a standalone `.sh` script that's invoked from both jobs? I think the only differences between them are the environment variables `PRIMER_SELECTOR`, `TY_MAX_PARALLELISM` and `TY_MEMORY_REPORT`, if I'm reading it correctly?

---

_@AlexWaygood reviewed on 2025-07-04 10:37_

thank you!

---

_Comment by @github-actions[bot] on 2025-07-04 21:25_

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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/primer/memory.txt`:4 on 2025-07-05 16:44_

sympy might be a good addition here, I think -- we have very high memory usage on it already

---

_@AlexWaygood approved on 2025-07-05 16:47_

Nice!

---

_Comment by @MichaReiser on 2025-07-07 08:42_

I'm a bit surprised that the memory usage results are non deterministic when running multi threaded (to such large degrees). The only two reasons I can think of are:

* AST GC 
* intermediate results from fixpoint cycles and the non-determinism comes from entering the cycles from different entry queries. 

Do we know which of the two is causing the flakes?

---

_@MichaReiser reviewed on 2025-07-07 08:45_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:299 on 2025-07-07 08:45_

I, at first, didn't understand what features is needed to replace this environment variable usage but I think I understand now. What we need is a `--quiet` or similar flag. Would you mind opening an issue in the ty repository for adding a flag to suppress diagnostic printing (and mention Ruff's `--quiet` and `--silent` flags)

---

_@MichaReiser approved on 2025-07-07 08:46_

---

_Merged by @ibraheemdev on 2025-07-07 16:17_

---

_Closed by @ibraheemdev on 2025-07-07 16:17_

---

_Branch deleted on 2025-07-07 16:17_

---

_@ibraheemdev reviewed on 2025-07-07 16:19_

---

_Review comment by @ibraheemdev on `crates/ty/src/lib.rs`:299 on 2025-07-07 16:19_

Created https://github.com/astral-sh/ty/issues/772

---

_Comment by @ibraheemdev on 2025-07-07 16:22_

> I'm a bit surprised that the memory usage results are non deterministic when running multi threaded (to such large degrees)

This is partly due to our rounding strategy being affected by very small changes if we get unlucky with the numbers. Now that we have a separate job we could just get the exact numbers manually. That said, on my machine I was seeing variance of ~10MB on projects with total memory usage in the hundreds of MB. It's possible that AST garbage collection is the culprit.

---
