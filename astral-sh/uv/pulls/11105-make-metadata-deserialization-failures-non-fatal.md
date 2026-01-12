```yaml
number: 11105
title: Make metadata deserialization failures non-fatal in the cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/de
created_at: 2025-01-30T17:35:38Z
updated_at: 2025-02-01T02:10:01Z
url: https://github.com/astral-sh/uv/pull/11105
synced_at: 2026-01-12T16:09:40Z
```

# Make metadata deserialization failures non-fatal in the cache

---

_@charliermarsh_

## Summary

If we fail to deserialize cached metadata in the cache, we should just ignore it, rather than failing.

Ideally, this never happens. If it does, it means we missed a cache version bump. But if it does happen, it should still be non-fatal.

Closes https://github.com/astral-sh/uv/issues/11043.

Closes https://github.com/astral-sh/uv/issues/11101.

## Test Plan

Prior to this PR, the following would fail:

- `uvx uv@0.5.25 venv --python 3.12 --cache-dir foo`
- `uvx uv@0.5.25 pip install ./scripts/packages/hatchling_dynamic --no-deps --python 3.12 --cache-dir foo`
- `uvx uv@0.5.18 venv --python 3.12 --cache-dir foo`
- `uvx uv@0.5.18 pip install ./scripts/packages/hatchling_dynamic --no-deps --python 3.12 --cache-dir foo`

We can't go back and fix 0.5.18, but this will prevent such regressions in the future.


---

_Label `bug` added by @charliermarsh on 2025-01-30 17:35_

---

_Label `cache` added by @charliermarsh on 2025-01-30 17:35_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 17:35_

---

_Review requested from @konstin by @charliermarsh on 2025-01-30 17:35_

---

_@charliermarsh reviewed on 2025-01-30 17:37_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:796 on 2025-01-30 17:37_

This is a bit of a bummer, since we're invalidating a TON of data. If we don't do this, though, then users running newer versions can hit these errors if they then check out older versions (since we'd be writing to `sdists-v6`, but those old versions would fail to read, since these errors are fatal right now).

It's a tradeoff. I'm undecided on what's best (bump or leave it as-is).


---

_@zanieb approved on 2025-01-30 17:41_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:796 on 2025-01-30 17:41_

I don't have strong feelings here. This feels vaguely correct. Are there other cache changes we've been delaying?

---

_@zanieb reviewed on 2025-01-30 17:41_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:796 on 2025-01-30 17:48_

I don't believe so...

---

_@charliermarsh reviewed on 2025-01-30 17:48_

---

_@charliermarsh reviewed on 2025-01-30 17:48_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:796 on 2025-01-30 17:48_

I think it's right to bump here. Correctness and errors should be given priority.

---

_Merged by @charliermarsh on 2025-01-30 17:48_

---

_Closed by @charliermarsh on 2025-01-30 17:48_

---

_Branch deleted on 2025-01-30 17:48_

---

_Comment by @zackees on 2025-02-01 00:17_

Just hit this bug too.

The uv cache purge fixed it.

Thanks for the fix getting merged in.



---

_Comment by @charliermarsh on 2025-02-01 00:20_

Did you hit this while running 0.5.26? Or 0.5.25?

---

_Comment by @zackees on 2025-02-01 02:10_

Sorry, not sure. It was an unversioned pip dependency.

---
