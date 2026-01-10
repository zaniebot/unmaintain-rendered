```yaml
number: 10622
title: Omit dynamic versions from the lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/dyn
created_at: 2025-01-15T02:01:28Z
updated_at: 2025-01-17T03:33:09Z
url: https://github.com/astral-sh/uv/pull/10622
synced_at: 2026-01-10T11:44:59Z
```

# Omit dynamic versions from the lockfile

---

_Pull request opened by @charliermarsh on 2025-01-15 02:01_

## Summary

This PR modifies the lockfile to omit versions for source trees that use `dynamic` versioning, thereby enabling projects to use dynamic versioning with `uv.lock`.

Prior to this change, dynamic versioning was largely incompatible with locking, especially for popular tools like `setuptools_scm` -- in that case, every commit bumps the version, so every commit invalidates the committed lockfile.

Closes https://github.com/astral-sh/uv/issues/7533.


---

_Review requested from @zanieb by @charliermarsh on 2025-01-15 03:03_

---

_Review requested from @konstin by @charliermarsh on 2025-01-15 03:03_

---

_Marked ready for review by @charliermarsh on 2025-01-15 03:04_

---

_@charliermarsh reviewed on 2025-01-15 03:05_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/snapshots/it__ecosystem__black-lock-file.snap`:182 on 2025-01-15 03:05_

This is correct: the `pyproject.toml` we have checked-in here uses dynamic versioning.

---

_Label `enhancement` added by @charliermarsh on 2025-01-15 03:07_

---

_@konstin approved on 2025-01-15 11:43_

---

_Merged by @charliermarsh on 2025-01-15 16:54_

---

_Closed by @charliermarsh on 2025-01-15 16:54_

---

_Branch deleted on 2025-01-15 16:54_

---

_Comment by @danielhollas on 2025-01-16 13:09_

Thank you for implementing this! :heart: 

Just a quick note that even with this change, `uv lock` still invokes the build backend for some reason. Is that expected?

We're hitting this over at https://github.com/astral-sh/uv-pre-commit/issues/35

---

_Comment by @charliermarsh on 2025-01-16 14:08_

There’s a TODO to avoid it, sorry!

---

_@jsirois reviewed on 2025-01-17 02:36_

---

_Review comment by @jsirois on `crates/uv-resolver/src/lock/mod.rs`:2463 on 2025-01-17 02:36_

This was a breaking change for those not specifying / bumping a `required-version`: lock gets modified by new uv to omit version for an editable project and then project dev with older uv chokes on the new lock when they pull. Presumably pre-1.0 this is to be expected though. I saw the changelog entry and even noticed the uv lock status fly-by noting a change but did not take this in for what it meant.

---

_@charliermarsh reviewed on 2025-01-17 03:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:2463 on 2025-01-17 03:33_

Yeah sorry, this was an oversight.

---
