```yaml
number: 15994
title: "Refresh lockfile when `--refresh` is provided (#15991)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/re
created_at: 2025-09-22T21:36:44Z
updated_at: 2025-09-23T11:25:15Z
url: https://github.com/astral-sh/uv/pull/15994
synced_at: 2026-01-12T16:12:03Z
```

# Refresh lockfile when `--refresh` is provided (#15991)

---

_@charliermarsh_

## Summary

If you provide `--refresh` to `uv lock`, we'll now always resolve (even though it might return the same result). This is also robust to `--locked` such that `--refresh --locked` will only fail if the lockfile changes.

Closes https://github.com/astral-sh/uv/issues/15997.


---

_Comment by @charliermarsh on 2025-09-22 21:52_

I will fix https://github.com/astral-sh/uv/issues/15997 first.

---

_Review requested from @zanieb by @charliermarsh on 2025-09-23 01:08_

---

_Review requested from @konstin by @charliermarsh on 2025-09-23 01:08_

---

_Label `enhancement` added by @charliermarsh on 2025-09-23 01:08_

---

_Marked ready for review by @charliermarsh on 2025-09-23 01:08_

---

_@charliermarsh reviewed on 2025-09-23 01:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:3250 on 2025-09-23 01:09_

I think this is _not_ necessary because we simplify the markers when comparing? I ran through the test cases from the linked PR (https://github.com/astral-sh/uv/pull/13635/files#diff-82edd36151736f44055f699a34c8b19a63ffc4cf3c86bf5fb34d69f8ac88a957) and they still pass.

---

_@charliermarsh reviewed on 2025-09-23 01:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:3250 on 2025-09-23 01:09_

\cc @Gankra 

---

_@charliermarsh reviewed on 2025-09-23 01:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:4806 on 2025-09-23 01:09_

We need to round-trip this marker so that it's identical to the marker you get back after deserializing.

---

_@zanieb reviewed on 2025-09-23 01:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:31742 on 2025-09-23 01:15_

You could / should assert the revision changes too, as that's a use-case for `--force`.

---

_@charliermarsh reviewed on 2025-09-23 01:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:222 on 2025-09-23 01:16_

I changed this because `LockResult::Changed` now means that there was _some_ change in the lockfile. It's more accurate now.

---

_@charliermarsh reviewed on 2025-09-23 01:47_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:31742 on 2025-09-23 01:47_

Done

---

_Review requested from @zanieb by @charliermarsh on 2025-09-23 01:54_

---

_@konstin approved on 2025-09-23 11:18_

---

_Merged by @charliermarsh on 2025-09-23 11:25_

---

_Closed by @charliermarsh on 2025-09-23 11:25_

---

_Branch deleted on 2025-09-23 11:25_

---
