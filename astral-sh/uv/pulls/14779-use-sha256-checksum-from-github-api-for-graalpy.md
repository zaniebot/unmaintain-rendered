```yaml
number: 14779
title: Use sha256 checksum from GitHub API for GraalPy releases
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: graalpy-checksum
created_at: 2025-07-21T06:11:07Z
updated_at: 2025-07-22T04:18:19Z
url: https://github.com/astral-sh/uv/pull/14779
synced_at: 2026-01-12T16:11:24Z
```

# Use sha256 checksum from GitHub API for GraalPy releases

---

_@j178_

## Summary

Follow #14078, use GitHub generated sha256 for GraalPy releases too.

## Test Plan

```console
uv run ./crates/uv-python/fetch-download-metadata.py
```


---

_@konstin approved on 2025-07-21 10:23_

---

_Label `internal` added by @konstin on 2025-07-21 10:23_

---

_Review requested from @zanieb by @konstin on 2025-07-21 10:23_

---

_Merged by @charliermarsh on 2025-07-21 12:35_

---

_Closed by @charliermarsh on 2025-07-21 12:35_

---

_Branch deleted on 2025-07-22 04:18_

---
