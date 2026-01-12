```yaml
number: 8598
title: Ignore empty groups when validating lock
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2024-10-26T17:07:29Z
updated_at: 2024-10-26T17:29:08Z
url: https://github.com/astral-sh/uv/pull/8598
synced_at: 2026-01-12T16:08:23Z
```

# Ignore empty groups when validating lock

---

_@charliermarsh_

## Summary

It turns out we were omitting empty dependency groups from the lockfile metadata, which was then causing us to reject locks when empty groups were defined.

We now include them (that section of the lock is meant to be a true representation of the metadata, and an empty-but-defined group is different from an absent group), though we can ignore them for validation, since it doesn't affect any behavior.

Closes https://github.com/astral-sh/uv/issues/8581.


---

_Label `bug` added by @charliermarsh on 2024-10-26 17:07_

---

_Merged by @charliermarsh on 2024-10-26 17:29_

---

_Closed by @charliermarsh on 2024-10-26 17:29_

---

_Branch deleted on 2024-10-26 17:29_

---
