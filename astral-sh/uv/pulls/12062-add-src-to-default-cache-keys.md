```yaml
number: 12062
title: "Add `src` to default cache keys"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2025-03-08T02:57:34Z
updated_at: 2025-03-17T21:56:12Z
url: https://github.com/astral-sh/uv/pull/12062
synced_at: 2026-01-10T11:10:39Z
```

# Add `src` to default cache keys

---

_Pull request opened by @charliermarsh on 2025-03-08 02:57_

## Summary

This has come up a few times, so it seems worth addressing. If you migrate from a flat layout to a `src` layout or vice versa, we now invalidate the package metadata.

Closes https://github.com/astral-sh/uv/issues/12047


---

_Label `bug` added by @charliermarsh on 2025-03-08 02:57_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-08 02:57_

---

_@charliermarsh reviewed on 2025-03-08 03:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:10625 on 2025-03-08 03:23_

Ugh... `setuptools` puts `{name}.egg-info` inside `src`, so it bumps the cache key here (since creating a directory within a directory bumps the parent's `mtime` and `ctime`).

I could just record directory _existence_ and have the same effect? There doesn't seem to be reliable way to get directory creation time (ignoring child contents) cross-platform.

---

_@charliermarsh reviewed on 2025-03-08 03:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:10625 on 2025-03-08 03:25_

Actually, I can probably use the inode number instead of `mtime`.

---

_Review comment by @jtfmumm on `crates/uv/tests/it/pip_install.rs`:9991 on 2025-03-17 07:59_

Maybe we should be testing these cases explicitly:
* Reinstall after adding `src`
* Reinstall after adding/removing a different directory that was specified in `cache-keys`

---

_@jtfmumm reviewed on 2025-03-17 07:59_

---

_@zanieb approved on 2025-03-17 18:37_

I think this change in default behavior makes sense

---

_Merged by @charliermarsh on 2025-03-17 21:56_

---

_Closed by @charliermarsh on 2025-03-17 21:56_

---

_Branch deleted on 2025-03-17 21:56_

---
