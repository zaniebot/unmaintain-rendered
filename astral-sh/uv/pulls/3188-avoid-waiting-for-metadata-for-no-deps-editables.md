```yaml
number: 3188
title: "Avoid waiting for metadata for `--no-deps` editables"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2024-04-22T16:19:03Z
updated_at: 2024-04-22T16:29:20Z
url: https://github.com/astral-sh/uv/pull/3188
synced_at: 2026-01-12T16:05:29Z
```

# Avoid waiting for metadata for `--no-deps` editables

---

_@charliermarsh_

## Summary

We don't emit a request for this, so we shouldn't wait for it either -- we already have the metadata!

Closes https://github.com/astral-sh/uv/issues/3184.


---

_Label `bug` added by @charliermarsh on 2024-04-22 16:19_

---

_Marked ready for review by @charliermarsh on 2024-04-22 16:19_

---

_Merged by @charliermarsh on 2024-04-22 16:29_

---

_Closed by @charliermarsh on 2024-04-22 16:29_

---

_Branch deleted on 2024-04-22 16:29_

---
