```yaml
number: 5446
title: Avoid canonicalizing executables on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/unc
created_at: 2024-07-25T15:00:30Z
updated_at: 2024-07-26T12:57:38Z
url: https://github.com/astral-sh/uv/pull/5446
synced_at: 2026-01-12T16:06:49Z
```

# Avoid canonicalizing executables on Windows

---

_@charliermarsh_

## Summary

If you have an executable path on a network share path (like `\\some-host\some-share\...\python.exe`), canonicalizing it adds the `\\?` prefix, but dunce cannot safely strip it.

This PR changes the Windows logic to avoid canonicalizing altogether. We don't really expect symlinks on Windows, so it seems unimportant to resolve them.

Closes: https://github.com/astral-sh/uv/issues/5440.


---

_Marked ready for review by @charliermarsh on 2024-07-26 01:15_

---

_Renamed from "Avoid UNC paths for executables" to "Avoid canonicalizing executables on Windows" by @charliermarsh on 2024-07-26 01:15_

---

_Label `bug` added by @charliermarsh on 2024-07-26 01:15_

---

_Label `windows` added by @charliermarsh on 2024-07-26 01:15_

---

_Review requested from @konstin by @charliermarsh on 2024-07-26 01:18_

---

_@konstin approved on 2024-07-26 09:08_

LGTM

---

_Comment by @paveldikov on 2024-07-26 12:38_

Verified ee1e20cbc solves issue for me.

---

_Merged by @charliermarsh on 2024-07-26 12:57_

---

_Closed by @charliermarsh on 2024-07-26 12:57_

---

_Branch deleted on 2024-07-26 12:57_

---

_Comment by @charliermarsh on 2024-07-26 12:57_

Let's give it a try.

---
