```yaml
number: 7139
title: "Invalidate cache when `--config-settings` change"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/config-settings
created_at: 2024-09-06T21:42:27Z
updated_at: 2024-09-10T01:49:17Z
url: https://github.com/astral-sh/uv/pull/7139
synced_at: 2026-01-12T16:07:42Z
```

# Invalidate cache when `--config-settings` change

---

_@charliermarsh_

## Summary

If `--config-settings` are provided, we cache the built wheels under one more subdirectory.

We _don't_ invalidate the actual source (i.e., trigger a re-download) or metadata, though -- those can be reused even when `--config-settings` change.

Closes https://github.com/astral-sh/uv/issues/7028.


---

_Label `bug` added by @charliermarsh on 2024-09-06 21:43_

---

_Label `cache` added by @charliermarsh on 2024-09-06 21:43_

---

_Review requested from @konstin by @charliermarsh on 2024-09-06 21:43_

---

_@konstin approved on 2024-09-09 15:35_

---

_Merged by @charliermarsh on 2024-09-10 01:49_

---

_Closed by @charliermarsh on 2024-09-10 01:49_

---

_Branch deleted on 2024-09-10 01:49_

---
