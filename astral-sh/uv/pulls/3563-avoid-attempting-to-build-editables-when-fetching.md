```yaml
number: 3563
title: Avoid attempting to build editables when fetching metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dep-editable
created_at: 2024-05-13T23:58:00Z
updated_at: 2024-05-14T00:03:55Z
url: https://github.com/astral-sh/uv/pull/3563
synced_at: 2026-01-10T14:37:54Z
```

# Avoid attempting to build editables when fetching metadata

---

_Pull request opened by @charliermarsh on 2024-05-13 23:58_

## Summary

If we see an editable as a dependency, we currently attempt to fetch its metadata, when we shouldn't.

Closes https://github.com/astral-sh/uv/issues/3562.

---

_Label `bug` added by @charliermarsh on 2024-05-13 23:58_

---

_Marked ready for review by @charliermarsh on 2024-05-13 23:58_

---

_Merged by @charliermarsh on 2024-05-14 00:03_

---

_Closed by @charliermarsh on 2024-05-14 00:03_

---

_Branch deleted on 2024-05-14 00:03_

---
