```yaml
number: 7443
title: Use consistent PyPI cache bucket
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/index-api
created_at: 2024-09-16T22:05:48Z
updated_at: 2024-09-16T23:33:33Z
url: https://github.com/astral-sh/uv/pull/7443
synced_at: 2026-01-10T12:53:47Z
```

# Use consistent PyPI cache bucket

---

_Pull request opened by @charliermarsh on 2024-09-16 22:05_

## Summary

All the registry wheels were getting cached under `index/b2a7eb67d4c26b82` rather than `pypi`, because we used `IndexUrl::Url` rather than `IndexUrl::from`.


---

_@zanieb approved on 2024-09-16 22:07_

---

_Label `bug` added by @charliermarsh on 2024-09-16 22:08_

---

_Label `cache` added by @charliermarsh on 2024-09-16 22:08_

---

_Merged by @charliermarsh on 2024-09-16 23:33_

---

_Closed by @charliermarsh on 2024-09-16 23:33_

---

_Branch deleted on 2024-09-16 23:33_

---
