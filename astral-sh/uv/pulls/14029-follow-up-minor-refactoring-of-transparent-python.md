```yaml
number: 14029
title: Follow-up minor refactoring of transparent Python upgrade code
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/review-transparent-python-upgrade
created_at: 2025-06-13T15:12:47Z
updated_at: 2025-06-13T17:05:39Z
url: https://github.com/astral-sh/uv/pull/14029
synced_at: 2026-01-12T16:10:58Z
```

# Follow-up minor refactoring of transparent Python upgrade code

---

_@jtfmumm_

This PR:
* renames `PythonMinorVersionLink::symlink_exists()` to `..::exists`
* avoids rediscovering installations by chaining together new installations with pre-existing installations when searching for the highest patch per minor version key
* renames a couple of related errors and updates their messages to be more specific
* renames `create_bin_link` to `create_link_to_executable`
* removes a constructor that is no longer used (`PythonMinorVersinLink::from_interpreter()`)


---

_Label `internal` added by @jtfmumm on 2025-06-13 15:12_

---

_Marked ready for review by @jtfmumm on 2025-06-13 15:21_

---

_@zanieb approved on 2025-06-13 16:39_

---

_@zanieb reviewed on 2025-06-13 16:40_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:512 on 2025-06-13 16:40_

I worry a little about making this conditional, because this feature is intended for marking specific tests, not for changing the behavior of the test suite as a whole.

---

_@jtfmumm reviewed on 2025-06-13 16:50_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:512 on 2025-06-13 16:50_

Interesting. It looks as though the tests pass without this when upgrade is in preview mode. I've removed this for now and will have to revisit when stabilizing transparent upgrades.

---

_Merged by @jtfmumm on 2025-06-13 17:05_

---

_Closed by @jtfmumm on 2025-06-13 17:05_

---

_Branch deleted on 2025-06-13 17:05_

---
