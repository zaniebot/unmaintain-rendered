```yaml
number: 9024
title: "Allow `--no-build` to validate lock"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/no-build
created_at: 2024-11-11T18:38:00Z
updated_at: 2024-11-11T19:02:39Z
url: https://github.com/astral-sh/uv/pull/9024
synced_at: 2026-01-12T16:08:37Z
```

# Allow `--no-build` to validate lock

---

_@charliermarsh_

## Summary

Just as we don't enforce tag compliance, we shouldn't enforce `--no-build` when validating the lockfile. If we end up building from source, the distribution database will correctly error.

Closes https://github.com/astral-sh/uv/issues/9016.


---

_Label `bug` added by @charliermarsh on 2024-11-11 18:40_

---

_Merged by @charliermarsh on 2024-11-11 19:02_

---

_Closed by @charliermarsh on 2024-11-11 19:02_

---

_Branch deleted on 2024-11-11 19:02_

---
