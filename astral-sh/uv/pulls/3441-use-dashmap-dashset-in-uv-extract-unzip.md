```yaml
number: 3441
title: "Use `dashmap::DashSet` in `uv_extract::unzip`"
type: pull_request
state: closed
author: DaniPopes
labels: []
assignees: []
base: main
head: unzip-dashmap
created_at: 2024-05-07T22:06:10Z
updated_at: 2024-05-09T14:52:45Z
url: https://github.com/astral-sh/uv/pull/3441
synced_at: 2026-01-12T16:05:38Z
```

# Use `dashmap::DashSet` in `uv_extract::unzip`

---

_@DaniPopes_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Use `dashmap::HashSet` over `Mutex<HashSet>`.
Suggested in https://github.com/astral-sh/uv/pull/3435#discussion_r1592917098.

This is a bit more complex that I thought: the original hashmap lock was held through the `create_dir_all` call, and if we naively swap it for dashmap this is not the case anymore which creates a race condition. See f9a5feaa5b39892364c3f8b63f9ba22aa7917a75.

I don't know if this is worth it, feel free to close if not.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-07 23:59_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-07 23:59_

---

_Review comment by @BurntSushi on `crates/uv-extract/src/stream.rs`:18 on 2024-05-08 11:49_

Why this change?

---

_Review comment by @BurntSushi on `crates/uv-extract/src/sync.rs`:12 on 2024-05-08 11:51_

Since we're using this as a set, I'd find it clearer to define this as a `FxDashSet<V>` and fix the value type to `()`.

---

_Review comment by @BurntSushi on `crates/uv-extract/src/sync.rs`:12 on 2024-05-08 11:53_

I think I'd also add a comment here saying why we aren't using a `DashSet` directly. I assume it's because a `DashSet` has no API for holding the lock in the case of a non-existent key.

---

_@BurntSushi reviewed on 2024-05-08 11:57_

I think a `Mutex<HashSet>` is simpler here, so I think I want to see a reason to use a `DashMap` here in lieu of the simpler thing. And in particular, since we're holding the lock across I/O boundaries anyway, it's not clear to me that a concurrent hashmap is going to help us much if at all.

@ibraheemdev Did you have any thoughts here?

---

_Comment by @ibraheemdev on 2024-05-08 22:17_

Hmm I agree that if we're holding the lock during the fs operation `DashMap` probably doesn't buy us much. We could use a `OnceMap` here to avoid contention on the lock, however, I'm not sure if the contention is a bottleneck given that the download still happens in parallel. If it's not I agree this is probably not worth it.

---

_Comment by @charliermarsh on 2024-05-09 14:42_

Alright, let's close for now then. Sorry for the churn @DaniPopes, appreciate your work!

---

_Closed by @charliermarsh on 2024-05-09 14:42_

---

_Branch deleted on 2024-05-09 14:52_

---
