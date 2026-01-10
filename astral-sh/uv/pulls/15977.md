```yaml
number: 15977
title: "fix: ensure temporary caches remain until process completion"
type: pull_request
state: open
author: monosans
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-09-22T05:00:17Z
updated_at: 2025-10-06T15:50:05Z
url: https://github.com/astral-sh/uv/pull/15977
synced_at: 2026-01-10T06:36:15Z
```

# fix: ensure temporary caches remain until process completion

---

_Pull request opened by @monosans on 2025-09-22 05:00_

Fixes #15967 


---

_Review comment by @konstin on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 12:37_

In this case, we still to drop (or more specifically, unlock) the locked file, while keeping the temp directory around

---

_@konstin reviewed on 2025-09-22 12:37_

---

_Review requested from @konstin by @monosans on 2025-09-22 14:16_

---

_@zanieb reviewed on 2025-09-22 14:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 14:17_

Why do we need to unlock it if we're the only one operating on it?

---

_@konstin reviewed on 2025-09-22 16:49_

---

_Review comment by @konstin on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 16:49_

If we keep it locked, it means that every `uv run` blocks any `uv cache` operation until it's done, this would be the strictest setting but we have talked in another thread about that we don't actually want to block parallel operations on the cache where possible.

---

_@konstin reviewed on 2025-09-22 16:57_

---

_Review comment by @konstin on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 16:57_

At least with `--link-mode reflink|hardlink`, I see no risk for the spawned process being affected by cache cleaning

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 17:00_

This is specifically with `--no-cache` in which a temporary dedicated cache is being used?

---

_@zanieb reviewed on 2025-09-22 17:00_

---

_@zanieb reviewed on 2025-09-22 17:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 17:01_

So I don't think that keeping the (temporary) cache locked would block any other operations.

---

_@zanieb reviewed on 2025-09-22 19:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:389 on 2025-09-22 19:03_

We'll need this same change in the project run command per https://github.com/astral-sh/uv/issues/15989

---

_@charliermarsh reviewed on 2025-09-22 19:07_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:395 on 2025-09-22 19:07_

If this is specifically for `--no-cache`, then I don't see why it's a problem to leave it locked.

---

_Comment by @zanieb on 2025-09-22 23:49_

I just removed these `drop` statements entirely in https://github.com/astral-sh/uv/pull/15990 but we can consider releasing the lock for cases where the cache is not used in `uv run`. We can't just handle temporary caches though, e.g., `uv cache clean` during a concurrent `uv run --script` would delete the script environment. I'm not sure what's best.

---

_Comment by @konstin on 2025-09-23 11:35_

We should add the test case either way, the cache logic we can tune based on how much we care about `uv cache` commands not being blocked.

---

_Comment by @konstin on 2025-10-06 14:55_

> I just removed these `drop` statements entirely in #15990 but we can consider releasing the lock for cases where the cache is not used in `uv run`. We can't just handle temporary caches though, e.g., `uv cache clean` during a concurrent `uv run --script` would delete the script environment. I'm not sure what's best.

Are there any cases we don't want to release the locks with `uv run`, given the problems in https://github.com/astral-sh/setup-uv/issues/588?

---

_Comment by @zanieb on 2025-10-06 15:50_

Yeah I already gave an example there: `uv run --script` is not concurrency-safe with `uv cache clean` if you release the lock.

---
