```yaml
number: 10599
title: Include build tag in rendered wheel filenames
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - no-build
assignees: []
merged: true
base: main
head: charlie/build-tag
created_at: 2025-01-14T14:44:27Z
updated_at: 2025-01-14T15:01:46Z
url: https://github.com/astral-sh/uv/pull/10599
synced_at: 2026-01-10T11:44:58Z
```

# Include build tag in rendered wheel filenames

---

_Pull request opened by @charliermarsh on 2025-01-14 14:44_

## Summary

I don't think this had an impact in practice, but it is "wrong" to omit these. Confirmed that the cache (for example) now includes the build tag (as in, `mkl_fft-1.3.8-72-cp310-cp310-manylinux2014_x86_64`).


---

_Label `no-build` added by @charliermarsh on 2025-01-14 14:44_

---

_Label `bug` added by @charliermarsh on 2025-01-14 14:44_

---

_Merged by @charliermarsh on 2025-01-14 15:01_

---

_Closed by @charliermarsh on 2025-01-14 15:01_

---

_Branch deleted on 2025-01-14 15:01_

---
