```yaml
number: 9030
title: "Revert `uv.lock` changes when `uv add` fails"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/add-lock
created_at: 2024-11-11T20:58:14Z
updated_at: 2024-11-12T03:38:06Z
url: https://github.com/astral-sh/uv/pull/9030
synced_at: 2026-01-12T16:08:37Z
```

# Revert `uv.lock` changes when `uv add` fails

---

_@charliermarsh_

## Summary

If a `uv add` fails at the sync stage, we need to clean up the changes to the `uv.lock`, since it might've been edited during in the lock stage (which, by necessity, succeeded). As-is, we revert the `pyproject.toml` but not the `uv.lock`, so the two are out-of-sync.

Closes https://github.com/astral-sh/uv/issues/9028.
Closes https://github.com/astral-sh/uv/issues/7992.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-11 21:01_

---

_Label `bug` added by @charliermarsh on 2024-11-11 21:01_

---

_@zanieb approved on 2024-11-12 02:28_

---

_Merged by @charliermarsh on 2024-11-12 03:38_

---

_Closed by @charliermarsh on 2024-11-12 03:38_

---

_Branch deleted on 2024-11-12 03:38_

---
