```yaml
number: 1261
title: Reduce stack sizes further and ignore remaining tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/even-more-stack-sizes
created_at: 2024-02-06T21:39:39Z
updated_at: 2024-02-06T22:08:19Z
url: https://github.com/astral-sh/uv/pull/1261
synced_at: 2026-01-12T16:04:32Z
```

# Reduce stack sizes further and ignore remaining tests

---

_@konstin_

This PR reduces the stack sizes a windows a little further using the stack traces from stack overflows combined with looking at the type sizes. Ultimately, it ignore the three remaining tests failing in debug on windows due to stack overflows to unblock `cargo test` for windows on CI.

444 tests run: 444 passed (39 slow), 1 skipped

---

_Review requested from @BurntSushi by @konstin on 2024-02-06 21:39_

---

_Review requested from @charliermarsh by @konstin on 2024-02-06 21:39_

---

_Label `internal` added by @konstin on 2024-02-06 21:39_

---

_Label `windows` added by @konstin on 2024-02-06 21:39_

---

_@charliermarsh reviewed on 2024-02-06 21:42_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:413 on 2024-02-06 21:42_

This is unchanged, right?

---

_@charliermarsh reviewed on 2024-02-06 21:42_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:365 on 2024-02-06 21:42_

What's the motivation for splitting these up?

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:413 on 2024-02-06 21:42_

Yes, this is the rustrover extract refactoring with the function signature fixed up

---

_@konstin reviewed on 2024-02-06 21:42_

---

_@konstin reviewed on 2024-02-06 21:44_

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:365 on 2024-02-06 21:44_

This function showed up as being large with many locals, so i tried to split it up into functions which isolate the locals.

---

_Review comment by @BurntSushi on `crates/puffin-build/src/lib.rs`:365 on 2024-02-06 21:48_

Do you need `#[inline(never)]` to avoid them getting smushed back together? (Although if the stack size really becomes that large, probably the inliner won't inline them?)

---

_@BurntSushi approved on 2024-02-06 21:50_

One concern I have is that these `Box`/`boxed` things might be easy to get rid of in future refactorings, but I don't see an easy way around that. And I suppose if they are a real problem, then Windows CI should fail?

---

_@konstin reviewed on 2024-02-06 22:05_

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:365 on 2024-02-06 22:05_

I'd hope that in debug mode it doesn't get inlined, which is where we have the stack overflows. In release mode i'm happy if it gets inlined.

---

_Comment by @konstin on 2024-02-06 22:07_

> One concern I have is that these Box/boxed things might be easy to get rid of in future refactorings, but I don't see an easy way around that. And I suppose if they are a real problem, then Windows CI should fail?

We could consider adding CI with an artificially smaller stack size to ensure the maximum stack sizes don't grow, but since this is a problem that exclusively affects debug mode i'm fine just gating this on `cargo test` on windows passing.

---

_Comment by @konstin on 2024-02-06 22:08_

I'll do a follow up PR once windows CI has `cargo test` integration to try to reactivate those tests.

---

_Merged by @konstin on 2024-02-06 22:08_

---

_Closed by @konstin on 2024-02-06 22:08_

---

_Branch deleted on 2024-02-06 22:08_

---
