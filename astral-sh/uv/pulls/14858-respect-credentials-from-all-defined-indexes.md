```yaml
number: 14858
title: Respect credentials from all defined indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tool-index
created_at: 2025-07-23T21:13:52Z
updated_at: 2025-07-23T21:23:52Z
url: https://github.com/astral-sh/uv/pull/14858
synced_at: 2026-01-10T06:53:02Z
```

# Respect credentials from all defined indexes

---

_Pull request opened by @charliermarsh on 2025-07-23 21:13_

## Summary

The core problem here is that `allowed_indexes` only includes at most one "default" index. This is problematic for tool upgrades, since the index in the receipt will be marked as default, but credentials will be omitted; if credentials are then defined in a `uv.toml`, we'll never look at those, since that will _also_ be marked as default, and we only look at the first default.

Instead, we should consider all defined indexes in priority order.

Closes https://github.com/astral-sh/uv/issues/14806.


---

_Label `bug` added by @charliermarsh on 2025-07-23 21:13_

---

_Marked ready for review by @charliermarsh on 2025-07-23 21:13_

---

_Merged by @charliermarsh on 2025-07-23 21:23_

---

_Closed by @charliermarsh on 2025-07-23 21:23_

---

_Branch deleted on 2025-07-23 21:23_

---
