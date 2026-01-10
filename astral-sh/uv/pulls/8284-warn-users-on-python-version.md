```yaml
number: 8284
title: "Warn users on \"~=\" python-version"
type: pull_request
state: closed
author: ianpaul10
labels: []
assignees: []
base: main
head: feat/warn-on-tilde-eq
created_at: 2024-10-17T10:29:13Z
updated_at: 2025-05-27T16:16:21Z
url: https://github.com/astral-sh/uv/pull/8284
synced_at: 2026-01-10T11:10:34Z
```

# Warn users on "~=" python-version

---

_Pull request opened by @ianpaul10 on 2024-10-17 10:29_

## Summary

Closes #7426

Second part of #7426, that checks if the python-version uses a tilde eq without a patch version (e.g. `~=3.11`, which will be interpreted as `>=3.10, ==3.*`) and warns the user, asking them to specify a patch version.

## Test Plan

Unit tests were added for `is_tilde_exact_without_patch`


---

_@ianpaul10 reviewed on 2024-10-17 10:30_

---

_Review comment by @ianpaul10 on `crates/uv-resolver/src/requires_python/tests.rs`:166 on 2024-10-17 10:30_

While unlikely, this sort of version should not throw a warning, correct?

---

_Assigned to @zanieb by @zanieb on 2024-10-17 13:44_

---

_Renamed from "Feat/warn on tilde eq" to "Warn usre on "~=" python-version" by @ianpaul10 on 2024-10-18 01:55_

---

_Renamed from "Warn usre on "~=" python-version" to "Warn users on "~=" python-version" by @ianpaul10 on 2024-10-18 01:55_

---

_@zanieb reviewed on 2024-11-12 14:00_

---

_Review comment by @zanieb on `crates/uv-resolver/src/requires_python/tests.rs`:166 on 2024-11-12 14:00_

cc @konstin 

---

_@zanieb reviewed on 2024-11-12 14:03_

---

_Review comment by @zanieb on `crates/uv-resolver/src/requires_python/tests.rs`:166 on 2024-11-12 14:03_

(I think not)

---

_Comment by @zanieb on 2024-11-12 14:13_

Hey @ianpaul10, I updated this with `main` and added some test cases and it looks like it's not working. It might be transformed before it gets to your logic? Like, when I use `~=3.12` the logs show 

> Using Python request `>=3.12, <4` from `requires-python` metadata 

cc @BurntSushi 

---

_@konstin reviewed on 2024-11-12 15:34_

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python/tests.rs`:166 on 2024-11-12 15:34_

We can leave it without a warning. It's an odd version specifier to write and likely not what the user intended, but it's not one that specifically matches this case.

---

_Comment by @zanieb on 2025-05-27 16:16_

I'm going to close this as stale, but feel free to pick it back up!

---

_Closed by @zanieb on 2025-05-27 16:16_

---
