```yaml
number: 17474
title: "[red-knot] Add list of failing/slow ecosystem projects"
type: pull_request
state: merged
author: carljm
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: cjm/addprojects
created_at: 2025-04-18T22:30:46Z
updated_at: 2025-04-22T12:32:38Z
url: https://github.com/astral-sh/ruff/pull/17474
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add list of failing/slow ecosystem projects

---

_Pull request opened by @carljm on 2025-04-18 22:30_

## Summary

I ran red-knot on every project in mypy-primer. I moved every project where red-knot ran to completion (fast enough, and mypy-primer could handle its output) into `good.txt`, so it will run in our CI.

The remaining projects I left listed in `bad.txt`, with a comment summarizing the failure mode (a few don't fail, they are just slow -- on a debug build, at least -- or output too many diagnostics for mypy-primer to handle.)

We will now run CI on 109 projects; 34 are left in `bad.txt`.

The main question about this PR is whether running mypy-primer on 111 projects is too much for CI: does it take too long? does it make the mypy-primer output overwhelming? If it is too much, I can split `good.txt` into `run-in-ci.txt` and `good.txt`, and we can pick some subset to run in CI.

## Test Plan

CI on this PR!


---

_Label `red-knot` added by @AlexWaygood on 2025-04-18 22:32_

---

_Comment by @github-actions[bot] on 2025-04-18 22:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-18 22:41_

I'm pretty strongly in favour of running (a lot) more mypy_primer projects in CI. Apart from anything else, we've already had several cases where the mypy_primer workflow has caught new panics for us, which seem very important for us to know about.

It looks like the changes here mean that it now runs in six minutes, which _is_ quite a lot slower than the two minutes it took previously. We could consider sharding the mypy_primer workflow between several CI jobs and joining the artifacts afterwards, which is what both mypy and typeshed do in their mypy_primer workflows.

---

_Label `ci` added by @AlexWaygood on 2025-04-18 22:41_

---

_Marked ready for review by @carljm on 2025-04-18 23:51_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-18 23:51_

---

_Review requested from @sharkdp by @carljm on 2025-04-18 23:51_

---

_Review requested from @dcreager by @carljm on 2025-04-18 23:51_

---

_Comment by @carljm on 2025-04-18 23:52_

6 minutes is reasonable enough that I'm tempted to just land this as-is and move on; I don't want to spend a lot of time wrangling GitHub Actions. But maybe if it's easy to borrow the mypy/typeshed sharding setup, I could look at that. This does make mypy-primer the long pole in our CI (replacing windows and release-mode tests at 4 minutes.)

---

_Closed by @carljm on 2025-04-19 00:52_

---

_Reopened by @carljm on 2025-04-19 00:52_

---

_Comment by @carljm on 2025-04-19 13:43_

I tried to do some work towards sharding in https://github.com/astral-sh/ruff/pull/17475, but I feel like I've reached my timebox on that. I have the sharded runs working fine and uploading artifacts, but the "comment" workflow needs significant changes in order to download all the diff artifacts and combine them, and our current comment workflow is quite different from the typeshed/mypy ones. Compounding the difficulty, it appears that a workflow that is triggered on completion of another workflow (like the mypy primer comment one) only runs if it exists on the main branch of the repo, so it seems like debugging it would have to occur on main branch.

So at this point my proposal would be to merge this and accept that ecosystem checks take 6-8 minutes. Open to alternative proposals, including "put more work into sharding" or "reduce the project set."

---

_Comment by @carljm on 2025-04-19 14:00_

Hmm, scratch that; something caused the last run to take 20min and time out. Will need to dig into that before we can consider merging this.

---

_Converted to draft by @carljm on 2025-04-19 14:00_

---

_Renamed from "[red-knot] update mypy-primer projects list" to "[red-knot] Add list of failing/slow ecosystem projects" by @sharkdp on 2025-04-22 11:44_

---

_Marked ready for review by @sharkdp on 2025-04-22 12:05_

---

_@sharkdp approved on 2025-04-22 12:08_

@carljm Thank you for this. After updating mypy_primer to run from upstream (which also includes a lot of recent changes to project dependencies), I re-analyzed all projects here. See the commit history for a log of the changes that I did. I moved five projects to `bad.txt` (all `try_metaclass_` panics) which were recently part of `good.txt` (merged [separately](https://github.com/astral-sh/ruff/pull/17544)). I also moved four projects to `good.txt`. The hang on Tanjun is probably related to #17537, I reclassified it.

---

_Merged by @sharkdp on 2025-04-22 12:15_

---

_Closed by @sharkdp on 2025-04-22 12:15_

---

_Branch deleted on 2025-04-22 12:15_

---

_Comment by @MichaReiser on 2025-04-22 12:25_

I don't know if performance is still a concern and if it is mainly red knot or mypy primer being slow. If it still is a concern and it's mainly red knot, then consider creating a new build profile which performs a release build but disables LTO (you could even experiment with less aggressive optimizations). LTO tends to be the slowest step and this could be a good trade between a faster red-knot without spending too much time on compiling.

---

_Comment by @sharkdp on 2025-04-22 12:31_

> I don't know if performance is still a concern and if it is mainly red knot or mypy primer being slow.

The red knot compilation used to be the bottleneck (when we ran on a few projects only), which is why I made it a debug build. Now it's the setup (clone + dependency installation) + the actual execution of red knot, which is the bottleneck. So we could consider switching back to a release build, with the disadvantage that we would have worse backtraces and no debug-assertions.



> If it still is a concern and it's mainly red knot, then consider creating a new build profile which performs a release build but disables LTO (you could even experiment with less aggressive optimizations). LTO tends to be the slowest step and this could be a good trade between a faster red-knot without spending too much time on compiling.

:+1: 



---

_Comment by @MichaReiser on 2025-04-22 12:32_

>  with the disadvantage that we would have worse backtraces and no debug-assertions.

We could enable debug assertions, similar to the `profiling` profile

---
