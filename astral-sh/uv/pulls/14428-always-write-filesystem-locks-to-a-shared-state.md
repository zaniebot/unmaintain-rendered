```yaml
number: 14428
title: Always write filesystem locks to a shared state directory
type: pull_request
state: open
author: charliermarsh
labels:
  - breaking
assignees: []
base: main
head: charlie/shared-locks
created_at: 2025-07-02T19:42:12Z
updated_at: 2025-10-12T21:17:03Z
url: https://github.com/astral-sh/uv/pull/14428
synced_at: 2026-01-12T16:11:12Z
```

# Always write filesystem locks to a shared state directory

---

_@charliermarsh_

## Summary

We use filesystem locks in a variety of places. For example, we tend to lock a virtual environment if we're running a command that might modify it. Right now, those locks tend to be written in the same "location" as whatever they lock (e.g., we write a filesystem lock at `.venv/.lock`), unless we can't do that for whatever reason, in which case we write to a content-addressed path within the user's temporary directory. This causes a few problems: (1) we try to write to locations that the user may have marked as read-only (like a `.venv` with `uv run` or similar); (2) we leave `.lock` files allover user-space.

This PR instead moves all filesystem locks into a shared locks directory, which the user can also configure via `UV_LOCK_DIR`.

This is breaking in two ways:

1. If users use older uv versions with versions released after this change, the two versions won't be concurrency-safe with one another, since they'll use different locations with the locks. I think that's fine, but it is arguably breaking.
2. If users have marked certain directories under an allowlist (e.g., allow `.venv` to be written within an otherwise read-only filesystem), they'll now need to allow writes to the lock directory. Again, I think this is fine (and an improvement), but it may cause issues for some users in specific setups.

(Regarding (2): such failures are now treated as non-fatal anyway.)


---

_Added to milestone `v0.8.0` by @charliermarsh on 2025-07-02 20:08_

---

_Label `breaking` added by @charliermarsh on 2025-07-02 20:08_

---

_Marked ready for review by @charliermarsh on 2025-07-02 20:54_

---

_Review requested from @oconnor663 by @charliermarsh on 2025-07-02 20:55_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-02 20:55_

---

_@oconnor663 reviewed on 2025-07-02 21:41_

---

_Review comment by @oconnor663 on `crates/uv-lock/src/lib.rs`:20 on 2025-07-02 21:41_

Should paths be `canonicalize`d before hashing them?

---

_Review comment by @oconnor663 on `crates/uv-lock/src/lib.rs`:74 on 2025-07-02 21:45_

Taking `self` here means that in some sense we need to reason about a `FilesystemLocks` instance that has not had `.init()` called on it. I usually prefer to move initialization logic into the constructor, so that if you have an instance, you never need to ask whether it's been initialized.

---

_Review comment by @oconnor663 on `crates/uv-lock/src/lib.rs`:41 on 2025-07-02 21:54_

Making this a `LazyLock` of `Result` makes it kind of awkward to access the value, especially because `io::Error` isn't `Copy`/`Clone`. `LazyLock` doesn't really let us initialize things fallibly, so sometimes I prefer a pattern like this:

```rust
struct Thing;

impl Thing {
    fn fallible_new() -> io::Result<Self> {
        Ok(Self)
    }
}

fn get_the_global_thing() -> io::Result<&'static Thing> {
    static THE_THING: OnceLock<Thing> = OnceLock::new();
    if let Some(thing) = THE_THING.get() {
        return Ok(thing);
    }
    // We need to initialize THE_THING.
    let new_thing = Thing::fallible_new()?;
    // Initialization succeeded, now set the OnceLock.
    // If some other thread raced with us to set it, that's fine.
    _ = THE_THING.set(new_thing);
    Ok(THE_THING.get().unwrap())
}
```

(I think it's fine for two threads to race to init here, but when it's not fine you can add a second function-local `static INIT_MUTEX: Mutex<()> = Mutex::new(())` that you double-check-lock whenever the first `get` returns `None`.)

---

_Review comment by @oconnor663 on `crates/uv-lock/src/lib.rs`:62 on 2025-07-02 21:56_

Is number 3 implemented here?

Also in general, how do we feel about putting lockfiles in a directory that doesn't get cleaned automatically? Are they small enough that it pretty much doesn't matter? Would we plan to add cleanup later?

---

_@oconnor663 reviewed on 2025-07-02 21:56_

---

_@charliermarsh reviewed on 2025-07-02 22:28_

---

_Review comment by @charliermarsh on `crates/uv-lock/src/lib.rs`:62 on 2025-07-02 22:28_

Yes, (3) happens in `StateStore::from_settings`. This is basically just a clone of `InstalledTools` but with less functionality and a different backing directory.

> Also in general, how do we feel about putting lockfiles in a directory that doesn't get cleaned automatically? Are they small enough that it pretty much doesn't matter? Would we plan to add cleanup later?

We might want to clear these out as part of `uv cache prune` or add periodic garbage collection. I think it's sort of fine for now, though.


---

_Review comment by @charliermarsh on `crates/uv-lock/src/lib.rs`:20 on 2025-07-03 19:19_

Hmm. I don't think so, because these paths don't have to exist?

---

_@charliermarsh reviewed on 2025-07-03 19:19_

---

_Review comment by @charliermarsh on `crates/uv-lock/src/lib.rs`:20 on 2025-07-03 19:42_

We could absolutize them?

---

_@charliermarsh reviewed on 2025-07-03 19:42_

---

_Review requested from @oconnor663 by @charliermarsh on 2025-07-03 19:42_

---

_@oconnor663 reviewed on 2025-07-03 23:26_

---

_Review comment by @oconnor663 on `crates/uv-lock/src/lib.rs`:20 on 2025-07-03 23:26_

Ah good point. I could imagine a routine that finds the longest path prefix that exists, canonicalizes _that_, and then appends the rest (and maybe asserts that there are no `.` or `..` components among the rest). But it's probably not worth the trouble? To care about this you'd have to like `mkdir foo && ln -s foo bar` and then try to lock both `foo/doesnt_exist_yet` and `bar/doesnt_exist_yet`. You _could_ but like, will anyone ever do it? Also even the "perfect" implementation could get confused if you raced it against that `ln` invocation.

Absolutifying sounds good if it's just a one line change?

---

_Removed from milestone `v0.8.0` by @zanieb on 2025-07-24 21:01_

---

_Added to milestone `v0.9.0` by @zanieb on 2025-07-24 21:01_

---

_Removed from milestone `v0.9.0` by @zanieb on 2025-10-12 21:17_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:17_

---
