```yaml
number: 4070
title: "Default to current Python minor if `Requires-Python` is absent"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/default-requires-python
created_at: 2024-06-05T20:18:41Z
updated_at: 2024-06-05T20:45:52Z
url: https://github.com/astral-sh/uv/pull/4070
synced_at: 2026-01-12T16:06:01Z
```

# Default to current Python minor if `Requires-Python` is absent

---

_@charliermarsh_

## Summary

If `Requires-Python` is omitted in `uv lock` or `uv run`, we now warn and default to `>=` the current minor version.

Closes https://github.com/astral-sh/uv/issues/4050.


---

_Label `preview` added by @charliermarsh on 2024-06-05 20:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-05 20:25_

---

_Review requested from @konstin by @charliermarsh on 2024-06-05 20:25_

---

_Marked ready for review by @charliermarsh on 2024-06-05 20:25_

---

_@zanieb approved on 2024-06-05 20:34_

Oo without an upper bound, spicy.

---

_Merged by @charliermarsh on 2024-06-05 20:45_

---

_Closed by @charliermarsh on 2024-06-05 20:45_

---

_Branch deleted on 2024-06-05 20:45_

---
