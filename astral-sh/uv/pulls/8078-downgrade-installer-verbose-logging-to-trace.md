```yaml
number: 8078
title: Downgrade installer verbose logging to trace
type: pull_request
state: merged
author: charliermarsh
labels:
  - tracing
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-10-10T09:31:41Z
updated_at: 2024-10-10T12:28:19Z
url: https://github.com/astral-sh/uv/pull/8078
synced_at: 2026-01-12T16:08:09Z
```

# Downgrade installer verbose logging to trace

---

_@charliermarsh_

## Summary

Now that `uv-install-wheel` output shows up in `--verbose`, lets leave `debug!` to logs that users might want to see. Logging _every_ file we install seems excessive.


---

_Review requested from @konstin by @charliermarsh on 2024-10-10 09:31_

---

_Label `tracing` added by @charliermarsh on 2024-10-10 09:31_

---

_@konstin approved on 2024-10-10 12:13_

---

_Merged by @charliermarsh on 2024-10-10 12:28_

---

_Closed by @charliermarsh on 2024-10-10 12:28_

---

_Branch deleted on 2024-10-10 12:28_

---
