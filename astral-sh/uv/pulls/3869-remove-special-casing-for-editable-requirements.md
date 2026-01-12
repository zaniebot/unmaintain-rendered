```yaml
number: 3869
title: Remove special-casing for editable requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/editable
created_at: 2024-05-27T21:08:19Z
updated_at: 2024-05-28T15:49:35Z
url: https://github.com/astral-sh/uv/pull/3869
synced_at: 2026-01-12T16:05:54Z
```

# Remove special-casing for editable requirements

---

_@charliermarsh_

## Summary

There are a few behavior changes in here:

- We now enforce `--require-hashes` for editables, like pip. So if you use `--require-hashes` with an editable requirement, we'll reject it. I could change this if it seems off.
- We now treat source tree requirements, editable or not (e.g., both `-e ./black` and `./black`) as if `--refresh` is always enabled. This doesn't mean that we _always_ rebuild them; but if you pass `--reinstall`, then yes, we always rebuild them. I think this is an improvement and is close to how editables work today.

Closes #3844.

Closes #2695.


---

_Label `internal` added by @charliermarsh on 2024-05-27 21:31_

---

_Marked ready for review by @charliermarsh on 2024-05-27 21:31_

---

_Review requested from @konstin by @charliermarsh on 2024-05-27 21:32_

---

_Review comment by @konstin on `crates/uv-cache/src/lib.rs`:929 on 2024-05-28 11:36_

The comment needs updating

---

_Review comment by @konstin on `crates/uv-cache/src/lib.rs`:230 on 2024-05-28 11:39_

That's the `Cache` instance and not the cache on disk, right?

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:229 on 2024-05-28 11:42_

Can we reuse one of the other `pyproject.toml` types for this?

---

_Review comment by @konstin on `crates/uv/tests/pip_sync.rs`:4212 on 2024-05-28 11:49_

Let's say i have some local packages i want to install as editables, and pypi deps i want to install with `--require-hashes`, does this mean i can't do this anymore?

---

_@konstin approved on 2024-05-28 11:49_

---

_@charliermarsh reviewed on 2024-05-28 13:04_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:4212 on 2024-05-28 13:04_

Yeah, can’t do this anymore. I can special-case it if we want? It’s consistent with pip so I wasn’t sure.

---

_@charliermarsh reviewed on 2024-05-28 13:05_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:230 on 2024-05-28 13:05_

Yeah

---

_@charliermarsh reviewed on 2024-05-28 15:46_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:229 on 2024-05-28 15:46_

I actually _like_ having granular structs for these... It ensures that we don't error when there are errors in irrelevant fields, it makes it clear which fields we depend on for the check, etc.

---

_Merged by @charliermarsh on 2024-05-28 15:49_

---

_Closed by @charliermarsh on 2024-05-28 15:49_

---

_Branch deleted on 2024-05-28 15:49_

---
