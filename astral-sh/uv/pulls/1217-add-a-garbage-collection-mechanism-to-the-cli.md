```yaml
number: 1217
title: Add a garbage collection mechanism to the CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/gc
created_at: 2024-02-01T04:29:36Z
updated_at: 2024-03-21T18:07:49Z
url: https://github.com/astral-sh/uv/pull/1217
synced_at: 2026-01-12T16:04:31Z
```

# Add a garbage collection mechanism to the CLI

---

_@charliermarsh_

## Summary

Detects unused cache entries, which can come in a few forms:

1. Directories that are out-dated via our versioning scheme.
2. Old source distribution builds (i.e., we have a more recent version).
3. Old wheels (stored in `archive-v0`, but not symlinked-to from anywhere in the cache).

Closes https://github.com/astral-sh/puffin/issues/1059.


---

_Label `enhancement` added by @charliermarsh on 2024-02-01 04:29_

---

_@charliermarsh reviewed on 2024-02-01 04:30_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:323 on 2024-02-01 04:30_

This phase is pretty bad (tightly coupled to the cache, but awkwardly so in that it doesn't _read_ the manifest files that point to the latest entry, it just looks for the most recent directory and deletes all the others).

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-01 04:48_

---

_Comment by @charliermarsh on 2024-02-01 04:49_

I'll also add some tests for this. Parts of it are really tedious to test, but it's probably important.

---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:297 on 2024-02-01 13:07_

I'd personally write this loop body like so:

```rust
if !entry.file_type().is_dir() {
    continue;
}
if !entry.path().join("manifest.msgpack").exists() {
    continue;
}
let Some(created) = fs::reader_dir(entry.path())?.filter(...).max() else { continue };
// then do the actual deletion
```

This avoids extra indentation and gives the code a bit more of a linear flow and is easier to read IMO.

---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:323 on 2024-02-01 13:09_

Is there a reason to not read the manifest files?

I think being tightly coupled here is okay, since this is the `puffin-cache` crate after-all. :)

(I do think our cache representation is a little leaky, but that's a problem for another day.)

---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:330 on 2024-02-01 13:10_

Does this need to have `contents_first(true)` enabled? I don't _think_ so?

---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:326 on 2024-02-01 13:11_

Out of curiosity, why do you use `FxHashSet` here? (My default is `HashSet` from the standard library.)

---

_Review comment by @BurntSushi on `crates/puffin/src/commands/clean.rs`:179 on 2024-02-01 13:12_

This is nice.

---

_@BurntSushi reviewed on 2024-02-01 13:13_

Nice!

I can see how testing this would be annoying. Maybe one way is to just explicitly build cache directories and then run `prune` and assert the expected result. But then your tests are tightly coupled with the cache representation.

---

_@charliermarsh reviewed on 2024-02-01 13:15_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:323 on 2024-02-01 13:15_

One reason is that the manifest files and readers are all in `puffin-distribution` (which depends on cache crate), but this code is in the cache crate itself :(

It might be necessary though. Removing the "wrong" directories here will break the cache, which is really bad.

---

_Comment by @charliermarsh on 2024-03-21 18:03_

I'm merging this for now without "Old source distribution builds (i.e., we have a more recent version)." That's more complex, and was delaying it, but this is already useful.

---

_Label `cli` added by @charliermarsh on 2024-03-21 18:03_

---

_Comment by @charliermarsh on 2024-03-21 18:07_

I'm not sure why `CI / check system | python3.13 on windows (pull_request)` is now failing, but it shouldn't be related to this PR. I'll follow-up separately.

---

_Merged by @charliermarsh on 2024-03-21 18:07_

---

_Closed by @charliermarsh on 2024-03-21 18:07_

---

_Branch deleted on 2024-03-21 18:07_

---
