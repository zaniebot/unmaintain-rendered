```yaml
number: 1653
title: "Add `CACHEDIR.TAG` to uv-created virtualenvs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/cachedir
created_at: 2024-02-18T15:31:32Z
updated_at: 2024-02-18T18:32:12Z
url: https://github.com/astral-sh/uv/pull/1653
synced_at: 2026-01-12T16:04:41Z
```

# Add `CACHEDIR.TAG` to uv-created virtualenvs

---

_@charliermarsh_

## Summary

Just as we mark virtualenvs as `gitignore`d by default, we should also mark them as `CACHEDIR.TAG`, to ensure that they aren't included in backups, etc.

Closes https://github.com/astral-sh/uv/issues/1648.

## Test Plan

Ran `cargo run venv` and:

```
❯ ls .venv
CACHEDIR.TAG bin          lib          pyvenv.cfg
```


---

_Label `enhancement` added by @charliermarsh on 2024-02-18 15:31_

---

_Renamed from "Add CACHEDIR.TAG to uv-created virtualenvs" to "Add `CACHEDIR.TAG` to uv-created virtualenvs" by @charliermarsh on 2024-02-18 15:31_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-18 15:31_

---

_Review requested from @konstin by @charliermarsh on 2024-02-18 15:31_

---

_@zanieb approved on 2024-02-18 17:15_

---

_Merged by @charliermarsh on 2024-02-18 18:32_

---

_Closed by @charliermarsh on 2024-02-18 18:32_

---

_Branch deleted on 2024-02-18 18:32_

---
