```yaml
number: 9679
title: "Omit Windows Store `python3.13.exe` et al"
type: pull_request
state: merged
author: charliermarsh
labels:
  - windows
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-12-06T12:40:08Z
updated_at: 2024-12-10T02:59:33Z
url: https://github.com/astral-sh/uv/pull/9679
synced_at: 2026-01-10T12:00:01Z
```

# Omit Windows Store `python3.13.exe` et al

---

_Pull request opened by @charliermarsh on 2024-12-06 12:40_

## Summary

I'm not sure why this hasn't come up before... But it looks like this method is only looking at `python.exe` and `python3.exe`? From the user screenshots, the `python3.12.exe` and `python3.13.exe` are also present, though.

Closes https://github.com/astral-sh/uv/issues/9667.

---

_Review requested from @zanieb by @charliermarsh on 2024-12-06 12:40_

---

_Label `windows` added by @charliermarsh on 2024-12-06 12:40_

---

_@charliermarsh reviewed on 2024-12-06 12:40_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1162 on 2024-12-06 12:40_

Technically, I think we could omit this check entirely, since the caller only passes discovered executables... Should I just remove it? _Just_ check that it's a `.exe`?

---

_Merged by @charliermarsh on 2024-12-10 02:59_

---

_Closed by @charliermarsh on 2024-12-10 02:59_

---

_Branch deleted on 2024-12-10 02:59_

---
