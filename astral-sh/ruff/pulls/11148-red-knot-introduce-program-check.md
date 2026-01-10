```yaml
number: 11148
title: "red-knot: Introduce `program.check`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-program-check
created_at: 2024-04-25T16:31:05Z
updated_at: 2024-04-27T12:15:49Z
url: https://github.com/astral-sh/ruff/pull/11148
synced_at: 2026-01-10T22:37:01Z
```

# red-knot: Introduce `program.check`

---

_Pull request opened by @MichaReiser on 2024-04-25 16:31_

This PR refactors the `main.rs` by:

* Extracting the `check` logic and moving it to `program.check`. There's some complication involved because we want the host environment to control how checks are scheduled (concurrently, or on the same thread). What I have now kind of works and isn't too much of a mess, but I feel like I manually implemented `Future`s. @BurntSushi do you know if it's possible to use `Future`s with out async? Yeah, it sounds stupid, hehe but what I want is something that starts some work and an orchestration thread can later poll the results without knowing if the computation runs on the same or another thread. 
* I refactored the `main` loop to prevent that we create a new orchestration thread for every loop. Instead, the orchestration thread is now started once and it sends messages to the main thread, telling it what the next operation is that it should perform (react to file changes, print diagnostics, check the program)



---

_Comment by @github-actions[bot] on 2024-04-25 18:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-04-26 07:03_

---

_Review comment by @MichaReiser on `crates/red_knot/src/files.rs`:35 on 2024-04-26 07:03_

We could by using an unsafe transmute similar to salsa. 

---

_Renamed from "red knot program check" to "red-knot: Introduce `program.check`" by @MichaReiser on 2024-04-26 07:05_

---

_Label `internal` added by @MichaReiser on 2024-04-26 07:09_

---

_Marked ready for review by @MichaReiser on 2024-04-26 07:10_

---

_Review requested from @carljm by @MichaReiser on 2024-04-26 08:58_

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:116 on 2024-04-26 21:54_

Why do we need to clone this when it isn't used anywhere else? Won't the original one just be immediately dropped unused?

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:93 on 2024-04-26 21:56_

nit, but at some point I'd move everything from here down out of `main.rs` and into a lib module. I assume `main.rs` is really just for temporary testing and should ultimately go away when we are integrated with ruff, so anything that is intended to survive integration with ruff should be in lib?

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:223 on 2024-04-26 22:38_

Let me see if I understand the division of responsibilities between `MainLoop` and `Orchestrator` threads. `MainLoop` does the actual checking, using a rayon thread pool to parallelize checking. The job of `Orchestrator` is to a) collect file changes from a notification source, debounce them, and then hand them off to `MainLoop`, which applies them to the `Program` and then re-checks, and b) receive check results (diagnostics) from the rayon thread running the actual check, and hand those off to `MainLoop` for display?

The term "Orchestrator" initially had me thinking it would actually coordinate the checking work, but it doesn't really do that; that's `RayonCheckScheduler`. `Orchestrator` is really just orchestrating at the higher level of check requests and results.

It's not clear to me yet how these pieces will fit together in an editor context when we are handling requests much smaller than "check the whole program."

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:239 on 2024-04-26 22:43_

I guess `pending_analysis` represents the fact that we know `MainLoop` is currently running an analysis. Would it make sense for each analysis request to have a unique ID that gets carried along with all the relevant messages (`CheckProgram`, `CheckProgramStarted`, `CheckProgramCompleted`, `CheckProgramCancelled`, etc) and also stored in the `PendingAnalysisState`, just to help us validate our assumptions about what corresponds to what, rather than implicitly relying on the two state machines always having their states matched up correctly?

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:153 on 2024-04-26 22:51_

This blocks until checking is all done, right? So then we'll go to the next tick and probably find a `CheckCompleted` message waiting for us already? (Or else just a new `ApplyChanges` and `CheckProgram`, if this check got cancelled.)

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:296 on 2024-04-26 22:56_

```suggestion
                        Ok(OrchestratorMessage::CheckProgramStarted {..}| OrchestratorMessage::CheckProgramCompleted(_) | OrchestratorMessage::CheckProgramCancelled) => unreachable!("No program check should be running while debouncing changes."),
```

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:48 on 2024-04-26 23:02_

It's not clear to me that `Workspace` shouldn't just be rolled directly into `Program` instead of being a separate struct. But maybe there will be benefits in having `Workspace` separate.

---

_Review comment by @carljm on `crates/red_knot/src/program/check.rs`:107 on 2024-04-26 23:07_

Are "later" and "earlier" reversed in this sentence? If queued checks run immediately, that suggests that earlier-scheduled checks will run first and block later-scheduled checks.

---

_Review comment by @carljm on `crates/red_knot/src/program/check.rs`:37 on 2024-04-26 23:09_

This name seems confusing -- it doesn't really clarify how it is different from `check_file`. It's more like `actually_check_file`. The fact that it takes a context is clear from the signature, doesn't really add anything in the name. Maybe just `do_check`?

Right now it doesn't even really need the context (`CheckFileTask::run` could check for cancellation), but in future when we do more complex and slower checks, we'll probably want to do more cancellation checks internally here, not just one at the start?

---

_@carljm approved on 2024-04-26 23:25_

This is great! Much easier to follow, I think I got my head around all of it.

---

_@MichaReiser reviewed on 2024-04-27 08:38_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:116 on 2024-04-27 08:38_

Nice catch. I refactored this code like 10 times and must have overlooked that the clone is no longer necessary. I'm surprised that clippy wasn't yelling at me.

---

_@MichaReiser reviewed on 2024-04-27 08:39_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:93 on 2024-04-27 08:39_

That's a good point. Although I think that `MainLooop` is something that will not be reused across hosts. The LSP has its own main loop. So the goal is to have as little as possible in main loop that isn't host specific.

But yes, we should move more into lib (and start splitting `red_knot` into multiple crates like `red_knot_syntax, `red_knot_semantic`, `red_knot_types`. 

---

_@MichaReiser reviewed on 2024-04-27 08:42_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:223 on 2024-04-27 08:42_

Yes, that's correct. The `Orchestrator` orchestrates the individual moving pieces. It needs to run on it' own thread so that it can react to incoming events. But it's the main loop that does the actual work

> It's not clear to me yet how these pieces will fit together in an editor context when we are handling requests much smaller than "check the whole program."

`program.check` should only recheck files that need rechecking since the last `program.check` call. That's what needs to be called inside of the LSPs pull diagnostics/push_diagnostics handlers.

> The term "Orchestrator" initially had me thinking it would actually coordinate the checking work, but it doesn't really do that; that's RayonCheckScheduler. Orchestrator is really just orchestrating at the higher level of check requests and results.

I'm open to better names for `Orchestrator`.

---

_@MichaReiser reviewed on 2024-04-27 08:43_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:239 on 2024-04-27 08:43_

I consider it a bug if the state machines are out of sync and would prefer if we panic over trying to make it work somehow (and leak memory). 

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:153 on 2024-04-27 08:45_

Yes, that's correct. Although the `ApplyChanges` might gets debounced first to wait for new incoming file change message. It would be nice if this could all be in a single place but the challenge I faced is that we need to have a single "input" on which the orchestrator thread can wait on (suspend). 

---

_@MichaReiser reviewed on 2024-04-27 08:45_

---

_@MichaReiser reviewed on 2024-04-27 08:47_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:48 on 2024-04-27 08:47_

Yeah, `workspace` might just be rolled into `Program`. Although it's somewhat a different entity in that it doesn't represent the program state, but the project state. For example, we could discover all files in a workspace (which is different from all files in a Program because the workspace does not include third party dependencies), what configurations exist etc independent of the program. 

---

_@MichaReiser reviewed on 2024-04-27 08:48_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:107 on 2024-04-27 08:48_

Lol, I think that's actually no longer true because new files always get queued at the end (by using a channel).

---

_@MichaReiser reviewed on 2024-04-27 08:49_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:37 on 2024-04-27 08:49_

My idea is to make cancellation checks on every query, and expose an API so that we can perform checks even inside of queries. 

---

_Merged by @MichaReiser on 2024-04-27 09:01_

---

_Closed by @MichaReiser on 2024-04-27 09:01_

---

_Branch deleted on 2024-04-27 09:01_

---

_@carljm reviewed on 2024-04-27 12:15_

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:239 on 2024-04-27 12:15_

That's reasonable. I wasn't suggesting we try to recover, more just that this could help clarify what happened in the bug case. But maybe tracing is sufficient for that. 

---
