```yaml
number: 11202
title: "[red-knot] Refactor `program.check` scheduling"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-executor
created_at: 2024-04-29T12:28:17Z
updated_at: 2024-04-30T07:28:22Z
url: https://github.com/astral-sh/ruff/pull/11202
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] Refactor `program.check` scheduling

---

_@MichaReiser_

## Summary

This PR refactors `program.check`, specifically the part around custom schedulers. 

What I found awkward in the old implementation is that `check` owned the orchestration. Normally, that's something the scheduler wants to take care of. Giving that control to the scheduler gives it more freedom how to do the scheduling. 

Inversing control further has the advantage that the `SingleThreadedScheduler` can use lock-free data structures exclusively (it had to communicate with the main thread before by using a channel). The `SingleThreadedScheduler` implementation now also looks roughly how I would have wanted to write it: a single queue with all the tasks and a loop that processes the tasks until done. 

I don't know if this is fundamentally better, but it feels less around the corner to me (although, it required some going around the corner to make it work :laughing:)

This PR also fixes an issue in the `MultithreadedExecutor` where it would dead lock when the open files is larger than the available parallelism. It also fixes that it uses the thread pool size (`current_num_threads`) over the system supported max number of threads (`max_num_threads`).

## Test Plan

I used the slow lint to test that cancellation works correctly. 

## Alternative design

I think it would still be possible for `program.check` to accept an `impl Executor` and we may want to do this in the future (and should only require making some methods public). For now, having an enum seems sufficient.


---

_Label `internal` added by @MichaReiser on 2024-04-29 12:28_

---

_Review requested from @carljm by @MichaReiser on 2024-04-29 12:35_

---

_@MichaReiser reviewed on 2024-04-29 12:35_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:147 on 2024-04-29 12:35_

It feels cleaner now that the `rayon::scope` is no longer leaking.

---

_@MichaReiser reviewed on 2024-04-29 12:45_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:147 on 2024-04-29 12:45_

We could possibly make this an enum now that `program.check` only accepts an enum over two variants. However, I want to keep this a trait for now until we know how many scheduling strategies we have. 

---

_Comment by @github-actions[bot] on 2024-04-29 12:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-04-29 13:35_

---

_@carljm approved on 2024-04-30 00:01_

Nice! This seems much cleaner to me; it basically fixes all the things that didn't feel quite right in the previous version.

---

_Merged by @MichaReiser on 2024-04-30 07:23_

---

_Closed by @MichaReiser on 2024-04-30 07:23_

---

_Branch deleted on 2024-04-30 07:23_

---
