```yaml
number: 11846
title: "Suggest `uv tool update-shell` in PowerShell"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/shell
created_at: 2025-02-28T02:24:32Z
updated_at: 2025-03-01T03:32:50Z
url: https://github.com/astral-sh/uv/pull/11846
synced_at: 2026-01-12T16:10:01Z
```

# Suggest `uv tool update-shell` in PowerShell

---

_@charliermarsh_

## Summary

We need to decouple the "Is this shell supported by `update-shell`?" logic from the "Does this shell have known configuration files?" logic, specifically for Windows, which we can always update but not via configuration files.

Closes https://github.com/astral-sh/uv/issues/11803.


---

_Label `bug` added by @charliermarsh on 2025-02-28 02:24_

---

_Label `windows` added by @charliermarsh on 2025-02-28 02:24_

---

_Marked ready for review by @charliermarsh on 2025-02-28 02:25_

---

_@konstin approved on 2025-02-28 10:34_

---

_Merged by @charliermarsh on 2025-03-01 03:32_

---

_Closed by @charliermarsh on 2025-03-01 03:32_

---

_Branch deleted on 2025-03-01 03:32_

---
