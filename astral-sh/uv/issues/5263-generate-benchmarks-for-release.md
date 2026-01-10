```yaml
number: 5263
title: Generate benchmarks for release
type: issue
state: closed
author: charliermarsh
labels:
  - documentation
  - benchmarks
assignees: []
created_at: 2024-07-21T18:36:45Z
updated_at: 2024-07-28T23:27:15Z
url: https://github.com/astral-sh/uv/issues/5263
synced_at: 2026-01-10T04:53:49Z
```

# Generate benchmarks for release

---

_Issue opened by @charliermarsh on 2024-07-21 18:36_

We're going to want a new round of benchmarks with some new comparisons.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-21 18:36_

---

_Label `documentation` added by @charliermarsh on 2024-07-21 18:36_

---

_Label `benchmarks` added by @charliermarsh on 2024-07-21 18:36_

---

_Comment by @charliermarsh on 2024-07-28 17:21_

For projects:

- Cold resolve without lockfile
- Warm resolve without lockfile
- Cold resolve with lockfile
- Warm resolve with lockfile
- Incremental resolve (add an entry to `pyproject.toml`)

For tools:

- Cold run
- Warm run
- Already-installed run


---

_Closed by @charliermarsh on 2024-07-28 23:27_

---
