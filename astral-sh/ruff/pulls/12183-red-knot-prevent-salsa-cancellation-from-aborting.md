```yaml
number: 12183
title: "[red-knot] Prevent salsa cancellation from aborting the program"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-cancelled
created_at: 2024-07-04T09:13:44Z
updated_at: 2024-07-09T13:52:47Z
url: https://github.com/astral-sh/ruff/pull/12183
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Prevent salsa cancellation from aborting the program

---

_@MichaReiser_

## Summary

This PR wraps the public-visible `Program` methods to use `Cancelled::catch` to prevent that any salsa cancellation tears down its thread. 

I honestly don't think this change is safe in Rust terms, but it is what rust-analyzer and other Salsa based compilers to (most implement the `UnwindSafe` marker trait but I prefer not to do this). 
I've opened a Zulip thread to get more guidance but, for now, I think this is sufficiently "safe" (I hate this).

## Test Plan

I ran `red_knot` with `RED_KNOT_SLOW_LINT=1` which adds an artificial 10s pause to the `lint` query. The query was correctly cancelled when I made changes to a file in the workspace directory without tearing down the entire rayon thread pool. 


---

_Review requested from @carljm by @MichaReiser on 2024-07-04 09:13_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-07-04 09:13_

---

_Label `red-knot` added by @MichaReiser on 2024-07-04 09:13_

---

_Renamed from "[red-knot] Prevent salsa cancellation to abort program" to "[red-knot] Prevent salsa cancellation from aborting the program" by @MichaReiser on 2024-07-04 09:15_

---

_Comment by @github-actions[bot] on 2024-07-04 09:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot/src/program/mod.rs`:62 on 2024-07-04 15:36_

```suggestion
        // For example, multi-threaded code inside `Program` that calls a query could panic and unwind, before
```

---

_@carljm approved on 2024-07-04 15:37_

Sigh. Even though it's a bit verbose, I really liked the explicit approach with a Result that we used in our own pseudo-Salsa before.

---

_@BurntSushi reviewed on 2024-07-08 17:11_

---

_Review comment by @BurntSushi on `crates/red_knot/src/program/mod.rs`:79 on 2024-07-08 17:11_

I don't have a ton of context here, but I'm not too concerned about this personally. Unwind Safety is not the same as Safety in Rust-speak. Like, there's no UB risk here. Violating unwind safety can lead to logic bugs around mutation or inconsistent state, but is never supposed to lead to UB. (I use the weasel word "supposed to" here because anyone can write unsound APIs in Rust that purportedly rely on unwind safety to avoid UB, but, well, that's unsound. And the fault is on the code relying on unwind safety to avoid UB and _not_ on the code using `AssertUnwindSafe`.) There have been rumblings about [deprecating unwind safety altogether](https://internals.rust-lang.org/t/pre-rfc-deprecating-unwindsafe/15974).

---

_Review comment by @BurntSushi on `crates/red_knot/src/program/mod.rs`:79 on 2024-07-08 17:12_

With that said, is this using panicking as a means of error handling? That seems like bad juju, but like, I have no context at all on which to judge that design.

---

_@BurntSushi reviewed on 2024-07-08 17:12_

---

_@MichaReiser reviewed on 2024-07-09 08:05_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:79 on 2024-07-09 08:05_

Thanks @BurntSushi. That's very helpful. I share some of the sentiment from the linked thread. It was very unclear to me how big of a problem it is. The compiler erroring makes it extremely scary. I now feel less bad about this.

> With that said, is this using panicking as a means of error handling? 

Unfortunately yes. Salsa uses panics to abort queries when some input has changed to not waste time computing outdated results to error out and to recover from cyclic queries. I would rather prefer a `Result`, but I can see how this would hurt ergonomics quite a bit. But maybe something to bring up again, although I think it's an unlikely change to be accepted because rust-analyzer would have to rewrite all their code to Result's. 

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-09 08:22_

---

_Merged by @MichaReiser on 2024-07-09 08:26_

---

_Closed by @MichaReiser on 2024-07-09 08:26_

---

_Branch deleted on 2024-07-09 08:26_

---

_@BurntSushi reviewed on 2024-07-09 13:52_

---

_Review comment by @BurntSushi on `crates/red_knot/src/program/mod.rs`:79 on 2024-07-09 13:52_

Ah I see. It circumvents the "cancellation" problem. Yeah, I can see how panics would be a very attractive solution to that problem.

IIRC, Go's runtime implements pre-emptive multi-tasking by spawning signals to interrupt CPU heavy code. It's not the _same_ problem as here, but it feels similarish. (I'm not suggesting the use of signals, but rather, pointing out the difficulty of arbitrary cancellation.)

---
