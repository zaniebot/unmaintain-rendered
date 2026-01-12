```yaml
number: 4914
title: "`uv python install` uses debug error"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
  - preview
assignees: []
created_at: 2024-07-09T05:23:44Z
updated_at: 2024-07-18T02:05:18Z
url: https://github.com/astral-sh/uv/issues/4914
synced_at: 2026-01-12T15:58:52Z
```

# `uv python install` uses debug error

---

_@charliermarsh_

```
❯ cargo run python install 3.12.4
Searching for Python versions matching: Python 3.12.4
error: No download found for request: PythonDownloadRequest { version: Some(MajorMinorPatch(3, 12, 4)), implementation: Some(CPython), arch: Some(Arch(Aarch64(Aarch64))), os: Some(Os(Darwin)), libc: Some(None)
```

---

_Label `error messages` added by @charliermarsh on 2024-07-09 05:23_

---

_Label `preview` added by @charliermarsh on 2024-07-09 05:23_

---

_Comment by @charliermarsh on 2024-07-09 05:42_

How _should_ this be displayed?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 23:40_

---

_Closed by @charliermarsh on 2024-07-18 02:05_

---

_Closed by @charliermarsh on 2024-07-18 02:05_

---
