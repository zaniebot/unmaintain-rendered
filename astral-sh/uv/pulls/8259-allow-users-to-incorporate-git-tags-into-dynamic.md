```yaml
number: 8259
title: Allow users to incorporate Git tags into dynamic cache keys
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/git-tags
created_at: 2024-10-16T14:45:47Z
updated_at: 2024-10-16T15:13:31Z
url: https://github.com/astral-sh/uv/pull/8259
synced_at: 2026-01-12T16:08:14Z
```

# Allow users to incorporate Git tags into dynamic cache keys

---

_@charliermarsh_

## Summary

You can now use `cache-keys = [{ git = { commit = true, tags = true } }]` to include both the current commit and set of tags in the cache key.

Closes https://github.com/astral-sh/uv/issues/7866.

Closes https://github.com/astral-sh/uv/issues/7997.


---

_Label `enhancement` added by @charliermarsh on 2024-10-16 14:45_

---

_Merged by @charliermarsh on 2024-10-16 15:13_

---

_Closed by @charliermarsh on 2024-10-16 15:13_

---

_Branch deleted on 2024-10-16 15:13_

---
