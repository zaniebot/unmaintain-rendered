---
number: 5242
title: "\"prefix not found\" in nested uv init"
type: issue
state: closed
author: bluss
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-20T10:59:41Z
updated_at: 2024-07-20T15:47:12Z
url: https://github.com/astral-sh/uv/issues/5242
synced_at: 2026-01-10T01:23:46Z
---

# "prefix not found" in nested uv init

---

_Issue opened by @bluss on 2024-07-20 10:59_

Using uv 0.2.27
platform: linux (x86_64)

Steps to reproduce:

1. uv init uvinit
2. cd uvinit
3. uv init subproject -v

```
DEBUG uv 0.2.27
warning: `uv init` is experimental and may change without warning
DEBUG Found project root: `/home/user/proj/uvinit`
DEBUG No workspace root found, using project root
error: prefix not found
```

"prefix not found" is the error message from StripPrefixError

---

_Label `bug` added by @charliermarsh on 2024-07-20 11:13_

---

_Label `preview` added by @charliermarsh on 2024-07-20 11:13_

---

_Referenced in [astral-sh/uv#5247](../../astral-sh/uv/pulls/5247.md) on 2024-07-20 13:52_

---

_Closed by @zanieb on 2024-07-20 15:47_

---

_Closed by @zanieb on 2024-07-20 15:47_

---
