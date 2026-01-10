```yaml
number: 15319
title: "Fall back on previous panic hook when not in `catch_unwind` wrapper"
type: pull_request
state: merged
author: dcreager
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: dcreager/thread-safe-panics
created_at: 2025-01-07T15:12:18Z
updated_at: 2025-01-08T16:34:52Z
url: https://github.com/astral-sh/ruff/pull/15319
synced_at: 2026-01-10T20:34:00Z
```

# Fall back on previous panic hook when not in `catch_unwind` wrapper

---

_Pull request opened by @dcreager on 2025-01-07 15:12_

This fixes #15317.  Our `catch_unwind` wrapper installs a panic hook that captures (the rendered contents of) the panic info when a panic occurs.  Since the intent is that the caller will render the panic info in some custom way, the hook silences the default stderr panic output.

However, the panic hook is a global resource, so if any one thread was in the middle of a `catch_unwind` call, we would silence the default panic output for _all_ threads.

The solution is to also keep a thread local that indicates whether the current thread is in the middle of our `catch_unwind`, and to fall back on the default panic hook if not.

## Test Plan

Artificially added an mdtest parse error, ran tests via `cargo test -p red_knot_python_semantic` to run a large number of tests in parallel.  Before this patch, the panic message was swallowed as reported in #15317.  After, the panic message was shown.

---

_Review requested from @carljm by @dcreager on 2025-01-07 15:12_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-07 15:12_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-07 15:12_

---

_Review requested from @sharkdp by @dcreager on 2025-01-07 15:12_

---

_Label `testing` added by @AlexWaygood on 2025-01-07 15:14_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-07 15:14_

---

_Comment by @github-actions[bot] on 2025-01-07 15:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-01-07 15:19_

Thanks for jumping on this so quickly!! I think @MichaReiser or @sharkdp will almost certainly be a better reviewer here, though -- this feels like it's out of my comfort zone 😆

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-01-07 15:19_

---

_@MichaReiser reviewed on 2025-01-07 16:00_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/panic.rs`:28 on 2025-01-07 16:00_

Using a `OnceLock` assumes that the global panic hook never changes. I guess that's sort of fine because it's true in all use cases we have today, but it might be surprising that `catch_unwind` only works until some magic point in time (when the panic hook gets replaced)

I also find it somewhat unfortunate that the panic hook will linger along after all threads completed. Could we have an atomic counter that counts how many threads are inside a `catch_unwind` and restore the panic handler once it reaches `0`?

---

_@dcreager reviewed on 2025-01-07 16:37_

---

_Review comment by @dcreager on `crates/ruff_db/src/panic.rs`:28 on 2025-01-07 16:37_

Another option — which I actually implemented first but then discarded because it changed the calling API more drastically — would be to make a wrapper type (`PanicHandler`) and move `catch_unwind` to be an instance method of that type.  Its `new` method would install the custom hook, and its `Drop` implementation would restore the previous hook.

That would still not protect you against someone else installing a different panic hook while you're in the middle of using `PanicHandler`.  But the lifetime of that handler would clearly delineate where you are expecting the custom behavior to happen.

Unfortunately that also doesn't seem to play nice with our test harness, since the most obvious place to create (and drop) the `PanicHandler` is in `red_knot_test::run`.  But that wouldn't install the hook once globally for all test cases — it would install/uninstall the hook on a per-test basis, just like before.  Ideally we'd create a single `PanicHandler` for the entire `cargo test` invocation — but I'm not aware of a way to do that, nor of how to then pass that handler into the individual test case functions.

> I also find it somewhat unfortunate that the panic hook will linger along after all threads completed. Could we have an atomic counter that counts how many threads are inside a `catch_unwind` and restore the panic handler once it reaches `0`?

I think there would be a race condition here, where we would uninstall the hook whenever there are 0 threads _currently_ in the middle of a `catch_unwind`, but there might be a test thread that's still running and just hasn't gotten to its `catch_unwind` call yet.  So we would have to check and reinstall the hook if needed.  (Or blindly always install it?)

In the end it seemed better to lean into the assumption that the custom hook is installed once for the duration of the process.  And the `OnceLock` part is to make sure that (a) the custom hook is not installed unless you need it, and (b) the caller doesn't have to worry about explicit or concurrent initialization.

I could document all of this more clearly — e.g. with a warning that it's not correct to use this `catch_unwind` with anything that tries to install a competing panic hook.

---

_@MichaReiser approved on 2025-01-08 08:09_

---

_@dcreager reviewed on 2025-01-08 16:12_

---

_Review comment by @dcreager on `crates/ruff_db/src/panic.rs`:28 on 2025-01-08 16:12_

> I could document all of this more clearly — e.g. with a warning that it's not correct to use this `catch_unwind` with anything that tries to install a competing panic hook.

I added documentation that we don't work if you try to install a competing panic hook, and also added some detection of if that happens.

---

_Merged by @dcreager on 2025-01-08 16:34_

---

_Closed by @dcreager on 2025-01-08 16:34_

---

_Branch deleted on 2025-01-08 16:34_

---
