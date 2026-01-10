```yaml
number: 17421
title: "Server: Use `min` instead of `max` to limit the number of threads"
type: pull_request
state: merged
author: jnooree
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: fix-nproc
created_at: 2025-04-16T08:09:31Z
updated_at: 2025-04-17T22:03:55Z
url: https://github.com/astral-sh/ruff/pull/17421
synced_at: 2026-01-10T19:33:02Z
```

# Server: Use `min` instead of `max` to limit the number of threads

---

_Pull request opened by @jnooree on 2025-04-16 08:09_

## Summary

Prevent overcommit by using max 4 threads as intended.

Unintuitively, `.max()` returns the maximum value of `self` and the argument (not limiting to the argument). To limit the value to 4, one needs to use `.min()`.

https://doc.rust-lang.org/std/cmp/trait.Ord.html#method.max

---

_Review requested from @carljm by @jnooree on 2025-04-16 08:09_

---

_Review requested from @MichaReiser by @jnooree on 2025-04-16 08:09_

---

_Review requested from @AlexWaygood by @jnooree on 2025-04-16 08:09_

---

_Review requested from @sharkdp by @jnooree on 2025-04-16 08:09_

---

_Review requested from @dcreager by @jnooree on 2025-04-16 08:09_

---

_Renamed from "fix: use max four threads, not at least four threads" to "[red-knot] [ruff] use max four threads, not at least four threads" by @jnooree on 2025-04-16 08:10_

---

_Review comment by @AlexWaygood on `crates/ruff/src/lib.rs`:228 on 2025-04-16 09:14_

Could you say a bit more about the problem this PR is trying to fix? According to this comment, it looks like the current way the code works is intentional?

---

_@AlexWaygood reviewed on 2025-04-16 09:14_

Thanks for the PR!

---

_@jnooree reviewed on 2025-04-16 09:21_

---

_Review comment by @jnooree on `crates/ruff/src/lib.rs`:228 on 2025-04-16 09:21_

Unintuitively, `.max()` returns the maximum value of self and the argument (*not* limiting to the argument). To limit the value to 4, one needs to use `.min()`.

---

_Review comment by @jnooree on `crates/ruff/src/lib.rs`:228 on 2025-04-16 09:40_

See, e.g., https://github.com/astral-sh/ruff/blob/a2a7b1e26897e0257434b74f5734df84d7e3d5af/crates/ruff_db/src/system/os.rs#L433

---

_@jnooree reviewed on 2025-04-16 09:40_

---

_@AlexWaygood reviewed on 2025-04-16 09:51_

---

_Review comment by @AlexWaygood on `crates/ruff/src/lib.rs`:228 on 2025-04-16 09:51_

Oh I see. Right, that does make sense. Thanks!

---

_Comment by @github-actions[bot] on 2025-04-16 09:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-16 09:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-04-16 10:04_

---

_Comment by @jnooree on 2025-04-16 10:42_

@AlexWaygood Could you merge this for me? I don't have write access to this repo.

---

_Comment by @AlexWaygood on 2025-04-16 10:46_

> I don't have write access to this repo.

I know! I'm not the only maintainer here, though, and wanted to give other maintainers some time to chime in if they wanted to. Your reasoning seems correct to me, but I'm not familiar with this area of the codebase :-)

---

_@carljm approved on 2025-04-16 17:36_

This looks clearly correct to me (in that it makes the behavior match the comments describing the intended behavior.) If it were just the red-knot server, I'd just merge it, but since it also impacts the ruff LSP, I'd rather have @dhruvmanila confirm.

---

_@dhruvmanila approved on 2025-04-17 19:59_

Nice catch! I'm not sure how this slipped by but thank you for noticing and fixing it.

---

_Label `server` added by @dhruvmanila on 2025-04-17 20:00_

---

_Renamed from "[red-knot] [ruff] use max four threads, not at least four threads" to "Server: Use `min` instead of `max` to limit the number of threads" by @dhruvmanila on 2025-04-17 20:01_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-17 20:02_

---

_Merged by @dhruvmanila on 2025-04-17 20:02_

---

_Closed by @dhruvmanila on 2025-04-17 20:02_

---

_Branch deleted on 2025-04-17 22:03_

---
