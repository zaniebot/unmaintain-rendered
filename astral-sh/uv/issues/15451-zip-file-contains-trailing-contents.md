---
number: 15451
title: ZIP file contains trailing contents
type: issue
state: closed
author: bra-fsn
labels:
  - bug
assignees: []
created_at: 2025-08-22T12:30:54Z
updated_at: 2025-08-22T13:50:08Z
url: https://github.com/astral-sh/uv/issues/15451
synced_at: 2026-01-10T01:25:56Z
---

# ZIP file contains trailing contents

---

_Issue opened by @bra-fsn on 2025-08-22 12:30_

### Summary

When trying to install a local package from an AWS Code Artifact repo, I'm getting this error:
```
  × Failed to download `s1-dataiku==0.1.dev1+g990d7941d`
  ├─▶ Failed to extract archive:
  │   s1_dataiku-0.1.dev1+g990d7941d-py3-none-any.whl
  ╰─▶ ZIP file contains trailing contents after the end-of-central-directory
      record
```
Doing a version bisecting I found out that 0.8.5 works, above that it fails.
I then came across the [release notes](https://github.com/astral-sh/uv/releases/tag/0.8.6), which recommends filing a bug report, so here it is. I hope this helps catching an edge case which might affect others as well, and it's not our fault. :D 

The file was built with (latest version of build)
```
python -m build --wheel
```
As it's a private package, I can send the file in private.

ps: it's possible that the package is built/compressed on arm64 and used on amd64 or vica versa, might be relevant.

### Platform

Linux 6.8.0-71-generic x86_64 GNU/Linux (Ubuntu 24.04)

### Version

uv 0.8.13 (problem appeared in 0.8.6

### Python version

3.12.11

---

_Label `bug` added by @bra-fsn on 2025-08-22 12:30_

---

_Comment by @charliermarsh on 2025-08-22 12:35_

Thanks, and sorry about that. Are you able to send me the package by email (charlie@astral.sh)?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-22 12:37_

---

_Comment by @charliermarsh on 2025-08-22 12:46_

Acknowledging receipt by email, thanks!

---

_Referenced in [astral-sh/uv#15452](../../astral-sh/uv/pulls/15452.md) on 2025-08-22 13:34_

---

_Closed by @charliermarsh on 2025-08-22 13:50_

---
