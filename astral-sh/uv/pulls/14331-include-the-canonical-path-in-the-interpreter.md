```yaml
number: 14331
title: Include the canonical path in the interpreter query cache key
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/canonical-interp
created_at: 2025-06-27T20:21:49Z
updated_at: 2025-06-30T15:39:48Z
url: https://github.com/astral-sh/uv/pull/14331
synced_at: 2026-01-12T16:11:09Z
```

# Include the canonical path in the interpreter query cache key

---

_@zanieb_

This fixes an obscure cache collision in Python interpreter queries, which we believe to be the root cause of CI flakes we've been seeing where a project environment is invalidated and recreated.

This work follows from the logs in [this CI run](https://github.com/astral-sh/uv/actions/runs/15934322410/job/44950599993?pr=14326) which captured one of the flakes with tracing enabled. There, we can see that the project environment is invalidated because the Python interpreter in the environment has a different version than expected:

```
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.12.9, skipping probing: .venv/bin/python3
DEBUG The interpreter in the project environment has different version (3.12.9) than it was created with (3.9.21)
```

(this message is updated to reflect #14329)

The flow is roughly:

- We create an environment with 3.12.9
- We query the environment, and cache the interpreter version for `.venv/bin/python`
- We create an environment for 3.9.12, replacing the existing one
- We query the environment, and read the cached information

The Python cache entries are keyed by the absolute path to the interpreter, and rely on the modification time (ctime, nsec resolution) of the canonicalized path to determine if the cache entry should be invalidated. The key is a hex representation of a u64 sea hasher output — which is very unlikely to collide.

After an audit of the Python query caching logic, we determined that the most likely cause of a collision in cache entries is that the modification times of underlying interpreters are identical. This seems pretty feasible, especially if the file system does not support nanosecond precision — though it appears that the GitHub runners do support it.

The fix here is to include the canonicalized path in the cache key, which ensures we're looking at the modification time of the _same_ underlying interpreter.

This will "invalidate" all existing interpreter cache entries but that's not a big deal.

This should also have the effect of reducing cache churn for interpreters in virtual environments. Now, when you change Python versions, we won't invalidate the previous cache entry so if you change _back_ to the old version we can re-use our cached information.

It's a bit speculative, since we don't have a deterministic reproduction in CI, but this is the strongest candidate given the logs and should increase correctness regardless.

Closes https://github.com/astral-sh/uv/issues/14160
Closes https://github.com/astral-sh/uv/issues/13744
Closes https://github.com/astral-sh/uv/issues/13745

Once it's confirmed the flakes are resolved, we should revert

- https://github.com/astral-sh/uv/pull/14275
- #13817

---

_Label `bug` added by @zanieb on 2025-06-27 20:21_

---

_@charliermarsh approved on 2025-06-27 20:54_

---

_Marked ready for review by @zanieb on 2025-06-27 21:03_

---

_Review requested from @konstin by @zanieb on 2025-06-27 23:22_

---

_@konstin approved on 2025-06-30 14:31_

---

_Merged by @zanieb on 2025-06-30 15:39_

---

_Closed by @zanieb on 2025-06-30 15:39_

---

_Branch deleted on 2025-06-30 15:39_

---
