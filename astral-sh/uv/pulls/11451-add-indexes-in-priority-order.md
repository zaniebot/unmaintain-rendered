```yaml
number: 11451
title: Add indexes in priority order
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sl
created_at: 2025-02-12T16:12:32Z
updated_at: 2025-02-12T17:14:11Z
url: https://github.com/astral-sh/uv/pull/11451
synced_at: 2026-01-12T16:09:51Z
```

# Add indexes in priority order

---

_@charliermarsh_

## Summary

We need to add indexes in the order in which they're respected by the resolver. Otherwise, we risk writing an index to the `pyproject.toml` that is canonically equal (but not verbatim equivalent) to the index we use during resolutin.

Closes https://github.com/astral-sh/uv/issues/11312.


---

_Label `bug` added by @charliermarsh on 2025-02-12 16:12_

---

_Comment by @charliermarsh on 2025-02-12 16:25_

This actually isn't right.

---

_Marked ready for review by @charliermarsh on 2025-02-12 16:48_

---

_Merged by @charliermarsh on 2025-02-12 17:14_

---

_Closed by @charliermarsh on 2025-02-12 17:14_

---

_Branch deleted on 2025-02-12 17:14_

---

_Renamed from "Add indexes in reverse-priority order" to "Add indexes in priority order" by @charliermarsh on 2025-02-12 17:14_

---
