```yaml
number: 16342
title: " Add a 5 min default timeout for deadlocks "
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/locked-file-timeout
created_at: 2025-10-17T11:53:11Z
updated_at: 2025-12-04T13:59:05Z
url: https://github.com/astral-sh/uv/pull/16342
synced_at: 2026-01-12T16:12:13Z
```

#  Add a 5 min default timeout for deadlocks 

---

_@konstin_

When a process is running and another calls `uv cache clean` or `uv cache prune` we currently deadlock - sometimes until the CI timeout (https://github.com/astral-sh/setup-uv/issues/588). To avoid this, we add a default 5 min timeout waiting for a lock. 5 min balances allowing in-progress builds to finish, especially with larger native dependencies, while also giving timely errors for deadlocks on (remote) systems.

Commit 1 is a refactoring.

This branch also fixes a problem with the logging where acquired and released resources currently mismatch:

```
DEBUG Acquired lock for `https://github.com/tqdm/tqdm`
DEBUG Using existing Git source `https://github.com/tqdm/tqdm`
DEBUG Released lock at `C:\Users\Konsti\AppData\Local\uv\cache\git-v0\locks\16bb813afef8edd2`
```



---

_Label `bug` added by @konstin on 2025-10-17 11:53_

---

_@konstin reviewed on 2025-10-17 13:33_

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:87 on 2025-10-17 13:33_

This should happen rarely and already involves waiting, so we can spawn a thread. I quickly looked into making it generally async but it didn't seem worth the churn.

---

_@zanieb reviewed on 2025-10-21 14:21_

---

_Review comment by @zanieb on `crates/uv-fs/src/locked_file.rs`:34 on 2025-10-21 14:21_

It'd be nice to have this to our standard environment variable parsing in `EnvironmentOptions` instead, I don't want to keep adding ad-hoc parsing like this.

If you want to defer it to reduce churn, that's okay — but we should add it the tracking issue and make sure it's moved.

---

_@zanieb reviewed on 2025-10-21 14:22_

---

_Review comment by @zanieb on `crates/uv-fs/src/locked_file.rs`:40 on 2025-10-21 14:22_

We might want to say "You can set ... to increase the timeout" instead of "Set" which makes it sounds like you _should_ do that as the solution.

---

_@zanieb reviewed on 2025-10-21 14:26_

---

_Review comment by @zanieb on `crates/uv-fs/src/locked_file.rs`:87 on 2025-10-21 14:26_

Hm, we only have a couple calls to the blocking / sync versions of the lock APIs. I'd be tempted to make them async.

I wonder if we should move this timeout handling to the API functions so we can `run_with_timeout` in the blocking / sync versions and just use an async timeout in the async versions? I'm wary of spawning a thread just for a timeout in the async case.

---

_Review comment by @zanieb on `crates/uv/tests/it/cache_clean.rs`:273 on 2025-10-21 14:28_

I think this is more complicated than it needs to be. We can just do

https://github.com/astral-sh/uv/blob/107d4e0ac71540059699b6136b000ba8a616ade2/crates/uv/tests/it/cache_clean.rs#L74

---

_@zanieb reviewed on 2025-10-21 14:28_

---

_@konstin reviewed on 2025-10-22 08:40_

---

_Review comment by @konstin on `crates/uv-fs/src/locked_file.rs`:34 on 2025-10-22 08:40_

I've added it to https://github.com/astral-sh/uv/issues/14720, do you want a separate tracking issue?

I had looked into parsing this centrally but the locks are called in a lot of locations including e.g. a `LazyLock` in a `Default` impl (https://github.com/astral-sh/uv/blob/7f38a8ac3bac1c2f53762369d8bdca76bb3a84dd/crates/uv-auth/src/middleware.rs#L79)

---

_@zanieb approved on 2025-10-22 18:34_

https://github.com/astral-sh/uv/pull/16342#discussion_r2448565978 is my main remaining caveat.

We should probably also add a note in https://docs.astral.sh/uv/concepts/cache since that's the main place this will be relevant.

---

_Label `bug` removed by @zanieb on 2025-10-22 18:34_

---

_Label `enhancement` added by @zanieb on 2025-10-22 18:34_

---

_Comment by @zanieb on 2025-10-22 18:35_

On the timing, I guess I might expect something like 60s rather than 5m? 5m is nice and conservative though, we could reduce it later once we see that 5m doesn't break anything

---

_@konstin reviewed on 2025-10-27 12:57_

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:1788 on 2025-10-27 12:57_

That's a bit more risky change because it assumes tests do not lock or spawn something in the background and then operate on Python versions.

---

_Comment by @konstin on 2025-10-27 13:07_

I rewrote it entirely async and removed the duplication between sync and async as well as shared and exclusive.

> On the timing, I guess I might expect something like 60s rather than 5m? 5m is nice and conservative though, we could reduce it later once we see that 5m doesn't break anything

I can see some (e.g. Rust) build taking >60s, so I'd like to go with a higher timeout.

---

_@konstin reviewed on 2025-10-28 11:07_

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:1788 on 2025-10-28 11:07_

The alternative is making every integration test async

---

_Review requested from @zanieb by @konstin on 2025-11-03 21:45_

---

_Comment by @konstin on 2025-11-03 21:53_

I've rebased on top of dropping fs2

---

_@zanieb approved on 2025-12-03 13:56_

---

_Merged by @konstin on 2025-12-04 13:59_

---

_Closed by @konstin on 2025-12-04 13:59_

---

_Branch deleted on 2025-12-04 13:59_

---
