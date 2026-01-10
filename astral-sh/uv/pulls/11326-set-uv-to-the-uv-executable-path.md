```yaml
number: 11326
title: "Set `UV` to the uv executable path"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: tracking/060
head: zb/uv-var
created_at: 2025-02-07T18:40:16Z
updated_at: 2025-02-13T23:41:54Z
url: https://github.com/astral-sh/uv/pull/11326
synced_at: 2026-01-10T11:10:35Z
```

# Set `UV` to the uv executable path

---

_Pull request opened by @zanieb on 2025-02-07 18:40_

Closes https://github.com/astral-sh/uv/issues/8775

---

_Label `enhancement` added by @zanieb on 2025-02-07 18:40_

---

_@zanieb reviewed on 2025-02-07 18:47_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:65 on 2025-02-07 18:47_

We do this eagerly / early so it's available in any child process we might spawn.

I think this is better than special-casing, e.g., `uv run`.

---

_Marked ready for review by @zanieb on 2025-02-07 18:48_

---

_Review requested from @Gankra by @zanieb on 2025-02-07 18:48_

---

_Review requested from @BurntSushi by @zanieb on 2025-02-07 18:48_

---

_Review requested from @charliermarsh by @zanieb on 2025-02-07 18:48_

---

_Review request for @BurntSushi removed by @zanieb on 2025-02-07 18:48_

---

_@BurntSushi reviewed on 2025-02-07 18:54_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:65 on 2025-02-07 18:54_

Note that in Rust 2024, this function will be [marked `unsafe`](https://doc.rust-lang.org/edition-guide/rust-2024/newly-unsafe-functions.html#stdenvset_var-remove_var).

What does special-casing entail?

---

_@zanieb reviewed on 2025-02-07 18:58_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:65 on 2025-02-07 18:58_

We'd need to implement passing this through to every subprocess call (or some subset of them).

I think it's safe here, since we're ~single threaded and we're the only one using this variable. We could move it before we start the tokio runtime for additional "safety", but it's more obvious to put it here.

---

_@zanieb reviewed on 2025-02-07 19:15_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:65 on 2025-02-07 19:15_

I moved it earlier and into an unsafe block

https://github.com/astral-sh/uv/pull/11326/commits/17aab2f1f5302b416e9c869949bed4f1631af285

---

_@zanieb reviewed on 2025-02-07 19:17_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:65 on 2025-02-07 19:17_

I think marking this as unsafe and putting it there is just "best practice". I have no practical concerns here.

---

_Review requested from @BurntSushi by @zanieb on 2025-02-07 19:45_

---

_Comment by @zanieb on 2025-02-07 19:59_

Technically, this could be breaking as we'll now overwrite `UV` if set by a parent (e.g., if the user has this set to something).

We could avoid setting it in that case? But it also seems correct to set it?

---

_@BurntSushi reviewed on 2025-02-07 20:01_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:01_

Nit: can move part of the comment above to a `SAFETY:` comment here right above `unsafe`? It's convention for discharging proof obligations when using `unsafe`. So basically:

```rust
// SAFETY: This is safe because we are running it
// early in `main` before spawning any threads.
```

And actually, if you can, I'd move this to be literally as early as possible. e.g., Before arg parsing. Like, it would be surprising if Clap started spinning up threads and mutating env vars, but it's not the craziest thing I've ever heard of.

---

_@BurntSushi reviewed on 2025-02-07 20:02_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:02_

Even putting it in actual `main` if possible. Otherwise you need to mark this `main` function as `unsafe` too.

---

_@zanieb reviewed on 2025-02-07 20:03_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:03_

My thinking there was... if we have something that needs to respect the `UV` from the parent.. that'd be the opportunity to do it. I think that's fine to defer until we have a use-case though.

---

_@BurntSushi reviewed on 2025-02-07 20:04_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:04_

See https://docs.rs/dtolnay/latest/dtolnay/macro._03__soundness_bugs.html for more about soundness. Basically, if this `main` function isn't marked as `unsafe` and it stays as-is, then one would call it "unsound" because this is a library routine that can't control where it's called.

---

_@zanieb reviewed on 2025-02-07 20:07_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:07_

Addressed the first comment in https://github.com/astral-sh/uv/pull/11326/commits/ede000d221f912fec6b694732aa51b88ebe706b9 — accidentally amended out of habit.

---

_@zanieb reviewed on 2025-02-07 20:09_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:09_

We.. could do that.. but then it'd break this functionality for someone that did call uv via `main` and we already do have this big warning

```
/// WARNING: This entry point is not recommended for external consumption, the
/// uv binary interface is the official public API. When using this entry
/// point, uv assumes it is running in a process it controls and that the
/// entire process lifetime is managed by uv. Unexpected behavior may be
/// encountered if this entry point is called multiple times in a single process.
```

Feeling hesitant to move it even further up.

---

_@BurntSushi reviewed on 2025-02-07 20:16_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:16_

That's fair, but we should still follow Rust's soundness rules IMO. Which means we mark this `main` as `unsafe`. Then you'll want to add this to the function docs on the library `main`:

```rust
/// # Safety
///
/// It is only safe to call this routine when it is known that multiple
/// threads are not running.
```

And then when we call this in actual `main`, we can copy your `SAFETY` comment to actual `main`:

```rust
// SAFETY: It is safe to call this routine here because
// we know only one thread is running.
```

And then your safety comment in the _library_ `main` gets changed to:

```rust
// SAFETY: The proof obligation must be satisfied by the caller.
```

That way, things stay sound.

---

_@BurntSushi approved on 2025-02-07 20:23_

Perfect. You made the soundness pedant inside of me happy. :-)

---

_@zanieb reviewed on 2025-02-07 20:27_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1870 on 2025-02-07 20:27_

Thanks again <3

---

_Label `breaking` added by @zanieb on 2025-02-07 20:27_

---

_Comment by @zanieb on 2025-02-07 20:27_

Going to consider this breaking due to the overwrite possibility and the change of our `pub main` to unsafe.

---

_Added to milestone `v0.6.0` by @zanieb on 2025-02-07 20:27_

---

_@Gankra approved on 2025-02-07 20:28_

---

_Merged by @zanieb on 2025-02-07 20:44_

---

_Closed by @zanieb on 2025-02-07 20:44_

---

_Branch deleted on 2025-02-07 20:44_

---
