---
number: 10323
title: UV install for existing python grows _sysconfigdata__linux_x86_64-linux-gnu.py
type: issue
state: closed
author: lfn3
labels:
  - bug
  - duplicate
assignees: []
created_at: 2025-01-06T09:33:53Z
updated_at: 2025-01-06T14:32:53Z
url: https://github.com/astral-sh/uv/issues/10323
synced_at: 2026-01-10T01:24:52Z
---

# UV install for existing python grows _sysconfigdata__linux_x86_64-linux-gnu.py

---

_Issue opened by @lfn3 on 2025-01-06 09:33_

Running `uv python install 3.10` results in fields like "BYTESTR_DEPS" in _sysconfigdata__linux_x86_64 to grow with increasing numbers of `\`s.

This is with UV 0.5.11, running on RHEL 8.

This was run with the --native-tls flag and ALL_PROXY env var set.

I can't extract log output from the host this is running on, but the output of --verbose only consists of 6 lines, including "Skipping installation of Python executables"

---

_Comment by @charliermarsh on 2025-01-06 14:32_

This was fixed in 0.5.12. You'll need to upgrade uv.

---

_Closed by @charliermarsh on 2025-01-06 14:32_

---

_Label `bug` added by @charliermarsh on 2025-01-06 14:32_

---

_Label `duplicate` added by @charliermarsh on 2025-01-06 14:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-06 14:32_

---
