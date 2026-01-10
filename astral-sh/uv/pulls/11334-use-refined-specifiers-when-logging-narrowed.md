```yaml
number: 11334
title: Use refined specifiers when logging narrowed Python range
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - tracing
assignees: []
merged: true
base: main
head: charlie/spec
created_at: 2025-02-07T22:21:24Z
updated_at: 2025-02-07T22:43:34Z
url: https://github.com/astral-sh/uv/pull/11334
synced_at: 2026-01-10T11:10:35Z
```

# Use refined specifiers when logging narrowed Python range

---

_Pull request opened by @charliermarsh on 2025-02-07 22:21_

## Summary

The narrowed ranges are now logged correctly:

```
DEBUG Narrowed `requires-python` bound to: >=3.10.0, <3.12
DEBUG Narrowed `requires-python` bound to: >=3.12
```

Closes https://github.com/astral-sh/uv/issues/11322.


---

_Label `bug` added by @charliermarsh on 2025-02-07 22:21_

---

_Label `tracing` added by @charliermarsh on 2025-02-07 22:21_

---

_@zanieb approved on 2025-02-07 22:22_

---

_Merged by @charliermarsh on 2025-02-07 22:43_

---

_Closed by @charliermarsh on 2025-02-07 22:43_

---

_Branch deleted on 2025-02-07 22:43_

---
