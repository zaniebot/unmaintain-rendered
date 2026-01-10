```yaml
number: 3646
title: "Add `UV_CONCURRENT_INSTALLS` variable in favor of `RAYON_NUM_THREADS`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - configuration
assignees: []
merged: true
base: main
head: install-concurrency
created_at: 2024-05-17T20:49:34Z
updated_at: 2024-05-18T03:12:37Z
url: https://github.com/astral-sh/uv/pull/3646
synced_at: 2026-01-10T14:32:20Z
```

# Add `UV_CONCURRENT_INSTALLS` variable in favor of `RAYON_NUM_THREADS`

---

_Pull request opened by @ibraheemdev on 2024-05-17 20:49_

## Summary

Continuation of https://github.com/astral-sh/uv/pull/3493. This gives us more flexibility in case we decide to move away from `rayon` in the future.

Should this be named something else? We also use `rayon` for unzipping files, and maybe other things in the future?

---

_Label `configuration` added by @ibraheemdev on 2024-05-17 20:49_

---

_Review requested from @zanieb by @ibraheemdev on 2024-05-17 20:49_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-05-17 20:49_

---

_Comment by @zanieb on 2024-05-17 21:10_

Are we always going to use a single global thread pool?

I guess... `UV_WORKER_THREADS` would be a more general name?

---

_Comment by @charliermarsh on 2024-05-17 21:11_

It seems like an implementation detail though. If, in the future, we use Rayon elsewhere, we may in fact want to use different pool sizes for different tasks -- or is that wrong?

---

_Comment by @ibraheemdev on 2024-05-17 21:23_

Yeah. For example we run some other file operations on tokios blocking pool. I think worker threads is a bit misleading because we already have download and build limits.

---

_Comment by @charliermarsh on 2024-05-17 21:24_

I think the current name is ok...

---

_@charliermarsh reviewed on 2024-05-17 21:33_

---

_Review comment by @charliermarsh on `README.md`:587 on 2024-05-17 21:33_

Does this no longer have any effect? (If it still has an effect, we should leave it here, even if `UV_CONCURRENT_INSTALLS` is recommended, since this is intended to document env variables that could affect behavior.)

---

_@charliermarsh reviewed on 2024-05-17 21:38_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/concurrency.rs`:35 on 2024-05-17 21:38_

Just confirming that this is the same as the Rayon default?

---

_Review comment by @ibraheemdev on `README.md`:587 on 2024-05-17 21:41_

It shouldn't have any effect because we're initialize the global thread pool manually.

---

_@ibraheemdev reviewed on 2024-05-17 21:41_

---

_@ibraheemdev reviewed on 2024-05-17 21:41_

---

_Review comment by @ibraheemdev on `crates/uv-configuration/src/concurrency.rs`:35 on 2024-05-17 21:41_

Yes, https://docs.rs/rayon-core/1.12.1/src/rayon_core/lib.rs.html#473.

---

_@charliermarsh approved on 2024-05-17 21:42_

Feel free to close that issue once merged IMO.

---

_Merged by @ibraheemdev on 2024-05-18 03:12_

---

_Closed by @ibraheemdev on 2024-05-18 03:12_

---
