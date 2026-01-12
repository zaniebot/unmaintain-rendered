```yaml
number: 6109
title: Treat Git sources as immutable in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
  - lock
assignees: []
merged: true
base: main
head: charlie/git-lock
created_at: 2024-08-15T12:58:54Z
updated_at: 2024-08-15T13:26:48Z
url: https://github.com/astral-sh/uv/pull/6109
synced_at: 2026-01-12T16:07:13Z
```

# Treat Git sources as immutable in lockfile

---

_@charliermarsh_

## Summary

We don't need to write metadata for Git sources, since we lock a specific SHA (and so the metadata is immutable).


---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-15 12:58_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-15 12:59_

---

_Label `preview` added by @charliermarsh on 2024-08-15 12:59_

---

_Label `lock` added by @charliermarsh on 2024-08-15 12:59_

---

_@BurntSushi approved on 2024-08-15 13:11_

Nice catch.

---

_Merged by @charliermarsh on 2024-08-15 13:26_

---

_Closed by @charliermarsh on 2024-08-15 13:26_

---

_Branch deleted on 2024-08-15 13:26_

---
