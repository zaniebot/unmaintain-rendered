```yaml
number: 3479
title: Filter irrelevant requirements from source annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/filter
created_at: 2024-05-09T04:10:08Z
updated_at: 2024-05-09T04:51:24Z
url: https://github.com/astral-sh/uv/pull/3479
synced_at: 2026-01-12T16:05:40Z
```

# Filter irrelevant requirements from source annotations

---

_@charliermarsh_

## Summary

If a requirement is omitted due to a marker expression, we shouldn't include it as the "source" of a package in the output.

For example, if your constraints include `pathspec ; python_version < '3.12'`, and you're on Python 3.12, we should _not_ include the constraint file as a "source" in the output annotations.


---

_Label `bug` added by @charliermarsh on 2024-05-09 04:10_

---

_Merged by @charliermarsh on 2024-05-09 04:51_

---

_Closed by @charliermarsh on 2024-05-09 04:51_

---

_Branch deleted on 2024-05-09 04:51_

---
