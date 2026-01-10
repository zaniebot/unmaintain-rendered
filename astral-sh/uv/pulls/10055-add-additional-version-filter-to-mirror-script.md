```yaml
number: 10055
title: Add additional version filter to mirror script.
type: pull_request
state: merged
author: bw513
labels:
  - internal
assignees: []
merged: true
base: main
head: mirror-script-filter-version
created_at: 2024-12-20T13:49:39Z
updated_at: 2024-12-20T18:51:05Z
url: https://github.com/astral-sh/uv/pull/10055
synced_at: 2026-01-10T11:44:32Z
```

# Add additional version filter to mirror script.

---

_Pull request opened by @bw513 on 2024-12-20 13:49_

## Summary

Adds regular expression based version filter to python mirror script.

## Test Plan

Manually using `uv run ./scripts/create-python-mirror.py --name cpython --arch x86_64 --os linux --version "3.13.\d+$"`

---

_@zanieb approved on 2024-12-20 18:50_

---

_Merged by @zanieb on 2024-12-20 18:50_

---

_Closed by @zanieb on 2024-12-20 18:50_

---

_Label `internal` added by @zanieb on 2024-12-20 18:51_

---
