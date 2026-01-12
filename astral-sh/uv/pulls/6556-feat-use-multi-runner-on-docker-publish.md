```yaml
number: 6556
title: "feat: use multi-runner on docker publish"
type: pull_request
state: merged
author: samypr100
labels:
  - releases
assignees: []
merged: true
base: main
head: multi-runner-docker-build
created_at: 2024-08-23T22:31:50Z
updated_at: 2024-08-24T01:03:58Z
url: https://github.com/astral-sh/uv/pull/6556
synced_at: 2026-01-12T16:07:25Z
```

# feat: use multi-runner on docker publish

---

_@samypr100_

## Summary

This PR parallelizes multi-platform builds using multiple workers (hence the new docker-build / docker-publish jobs), this seems to save about ~8 minutes.

This is partial work extracted from https://github.com/astral-sh/uv/pull/6053 than is standalone

---

_@zanieb approved on 2024-08-23 22:49_

---

_Label `release` added by @zanieb on 2024-08-23 22:49_

---

_Merged by @zanieb on 2024-08-23 23:50_

---

_Closed by @zanieb on 2024-08-23 23:50_

---

_Branch deleted on 2024-08-24 01:03_

---
