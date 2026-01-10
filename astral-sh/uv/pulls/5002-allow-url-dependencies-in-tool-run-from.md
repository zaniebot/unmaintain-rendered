```yaml
number: 5002
title: "Allow URL dependencies in tool run `--from`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/u
created_at: 2024-07-12T01:19:57Z
updated_at: 2024-07-12T01:31:08Z
url: https://github.com/astral-sh/uv/pull/5002
synced_at: 2026-01-10T13:42:52Z
```

# Allow URL dependencies in tool run `--from`

---

_Pull request opened by @charliermarsh on 2024-07-12 01:19_

## Summary

Converting to a lock requires that we generate hashes; but generating hashes isn't required here. So let's just use a different representation for the cache key.

Closes https://github.com/astral-sh/uv/issues/4990.


---

_Label `bug` added by @charliermarsh on 2024-07-12 01:20_

---

_Label `preview` added by @charliermarsh on 2024-07-12 01:20_

---

_Merged by @charliermarsh on 2024-07-12 01:31_

---

_Closed by @charliermarsh on 2024-07-12 01:31_

---

_Branch deleted on 2024-07-12 01:31_

---
