```yaml
number: 2259
title: "Close `RECORD` after reading entries during uninstall"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/install
created_at: 2024-03-07T02:37:54Z
updated_at: 2024-03-07T04:35:23Z
url: https://github.com/astral-sh/uv/pull/2259
synced_at: 2026-01-12T16:04:56Z
```

# Close `RECORD` after reading entries during uninstall

---

_@charliermarsh_

## Summary

It turns out that by keeping the `RECORD` file open, older versions of Windows mark it for deletion, but don't allow it to be deleted until it's closed. As such, we end up leaving the `.dist-info` directory around, since it appears non-empty; but once the program terminates, we _do_ delete `RECORD`, leaving it empty. This then creates the impression that a package exists where it does not.

Closes https://github.com/astral-sh/uv/issues/2074.

---

_Comment by @zanieb on 2024-03-07 03:40_

#1830 fyi I tried this previously

---

_Comment by @charliermarsh on 2024-03-07 03:43_

Thanks, this is specifically to reproduce a bug that only occurs on older Windows versions.

---

_Renamed from "Run tests on Windows 2019" to "Close `RECORD` after reading entries during uninstall" by @charliermarsh on 2024-03-07 04:31_

---

_Label `bug` added by @charliermarsh on 2024-03-07 04:31_

---

_Marked ready for review by @charliermarsh on 2024-03-07 04:31_

---

_Merged by @charliermarsh on 2024-03-07 04:35_

---

_Closed by @charliermarsh on 2024-03-07 04:35_

---

_Branch deleted on 2024-03-07 04:35_

---
