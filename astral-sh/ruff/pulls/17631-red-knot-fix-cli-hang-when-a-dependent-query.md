```yaml
number: 17631
title: "[red-knot] Fix CLI hang when a dependent query panics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/cli-hang
created_at: 2025-04-25T16:19:45Z
updated_at: 2025-04-28T07:25:45Z
url: https://github.com/astral-sh/ruff/pull/17631
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Fix CLI hang when a dependent query panics

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/17537

Red Knot's CLI sometimes *hangs* after encountering a panic. This was due to non-determinsim related to how panics are propagated by rayon and Salsa. 

* Our main loop spawns a job to run the check command to remain responsive to `Ctrl + C` commands and incoming file watcher changes
* This thread is spawned using `rayon::spawn`. Rayon terminates the entire process if the task scheduled with `rayon::spawn` panics (we don't have any other panic handling today). 
* This thread calls `db.check` right away which calls `project.check`, but is wrapped in a `salsa::Cancelled::catch`. I assumed that this only catches cancellations because of a pending write (e.g. a file watcher change) but it turns out, that it also catches panics if thread A depends on a query running in thread B and thread B panics. 
* The actual checking happens in `project.check` where we use a thread pool and spawn a task for every file. 
* `project.check` uses `rayon::scope`: Unlike `rayon::spawn`, it propagates panics instead of terminating the process. However, it propagates an arbitrary panic if two threads panic (which is the case if thread A depends on a query in thread B and B panics). 


What happened is that sometimes rayon propagated the `Cancelled::PropagatedPanic` (the panic raised by salsa in thread A that depends on the panicking query running in thread B) panic over the *actual* panic in thread B, which then got swallowed by our `Cancelled::catch` wrapper inside `db.check`. 


We should probably change Salsa to use a different mechanism to handle thread-dependent panics because this is a logical error, whereas a pending write is not. 

This PR fixes the hang by repeating the `Cancelled::catch` inside each spawned thread. This has the advantage that we propagate the *real* panic from thread B and never the *placeholder* `Cancelled::PropagatedPanic` from thread A (which doesn't contain any useful debug information). More specifically, we now catch panics inside `check_file_impl` and create a diagnostic for them.

```
error: panic: Panicked while checking `/Users/micha/astral/ecosystem/hydpy/hydpy/core/devicetools.py`: `dependency graph cycle querying try_metaclass_(Id(250b7)); set cycle_fn/cycle_initial to fixpoint iterate`
info: This indicates a bug in Red Knot.
info: If you could open an issue at https://github.com/astral-sh/ruff/issues/new?title=%5BRed%20Knot%20panic%5D, we'd be very appreciative!
```

Ideally, we'd capture the backtrace too but that's a) more complicated and b) mostly empty in production builds.


## Test Plan

I ran red knot on [hydpy](https://github.com/hydpy-dev/hydpy?rgh-link-date=2025-04-22T07%3A17%3A56.000Z) for a couple of minutes and I couldn't reproduce the hang anymore (normally reproduces after 30s or so)

---

_Label `red-knot` added by @MichaReiser on 2025-04-25 16:20_

---

_Comment by @github-actions[bot] on 2025-04-25 16:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-04-25 16:31_

---

_Review requested from @carljm by @MichaReiser on 2025-04-25 16:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-25 16:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-25 16:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-25 16:31_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-25 16:38_

---

_Converted to draft by @MichaReiser on 2025-04-25 16:41_

---

_Marked ready for review by @MichaReiser on 2025-04-25 17:13_

---

_Renamed from "[red-knot] Fix CLI hang when a dependend query panics" to "[red-knot] Fix CLI hang when a dependent query panics" by @AlexWaygood on 2025-04-25 17:15_

---

_Review comment by @carljm on `crates/red_knot_project/src/lib.rs`:607 on 2025-04-25 18:26_

nit: today we use `[red-knot]` prefix on all our issues and PRs, can we stay consistent with that? e.g. `[red-knot] panic:` maybe?

Of course we'll have to change this again soon :)

---

_@carljm approved on 2025-04-25 18:28_

Nice! Thank you so much for tracking this down.

The fix looks good, and also provides a better user experience for panics.

Is my understanding correct that this means new ecosystem panics will now again not show up as a failure in the ecosystem job, and instead just as a diagnostic output diff? I think that's OK, but it does mean we need to be careful to check ecosystem output on our diffs.

---

_Comment by @carljm on 2025-04-25 18:29_

Looks like clippy is not happy?

---

_Comment by @MichaReiser on 2025-04-25 20:18_

That's correct. But panics come first with Andrew's new sorting and should be easy to discover because of it (unless it gets truncated). 

---

_Merged by @MichaReiser on 2025-04-26 06:28_

---

_Closed by @MichaReiser on 2025-04-26 06:28_

---

_Branch deleted on 2025-04-26 06:28_

---

_Comment by @sharkdp on 2025-04-28 07:07_

> Is my understanding correct that this means new ecosystem panics will now again not show up as a failure in the ecosystem job, and instead just as a diagnostic output diff? I think that's OK, but it does mean we need to be careful to check ecosystem output on our diffs.

Maybe we could still exit with a code different from 1 (type checking failed) and 2 (some other red knot error), like before? In this case, it would still be easy to detect panics (require no changes in mypy_primer).

---

_Comment by @sharkdp on 2025-04-28 07:15_

> Maybe we could still exit with a code different from 1 (type checking failed) and 2 (some other red knot error), like before? In this case, it would still be easy to detect panics (require no changes in mypy_primer).

Oh, you already proposed that change in https://github.com/astral-sh/ruff/pull/17640. In that case, mypy_primer CI runs will still fail in case of panics.

Edit: well, not quite, still uses error code 2

---

_Comment by @MichaReiser on 2025-04-28 07:25_

> Edit: well, not quite, still uses error code 2

We could change the error code to something else but 2 is what Ruff/Red Knot already used for other errors. 

---
