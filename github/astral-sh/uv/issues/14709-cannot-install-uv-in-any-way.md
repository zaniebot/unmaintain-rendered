---
number: 14709
title: cannot install uv in any way
type: issue
state: closed
author: kevinnudt12
labels:
  - bug
assignees: []
created_at: 2025-07-18T09:58:48Z
updated_at: 2025-07-23T13:28:40Z
url: https://github.com/astral-sh/uv/issues/14709
synced_at: 2026-01-07T13:12:18-06:00
---

# cannot install uv in any way

---

_Issue opened by @kevinnudt12 on 2025-07-18 09:58_

### Summary

1. cargo install --git https://github.com/astral-sh/uv uv
    Updating git repository `https://github.com/astral-sh/uv`
error: could not find `uv` in https://github.com/astral-sh/uv with version `*`
2. curl install : no response
3. pipx install: couldn't locate

### Platform

clound ubuntu 24

### Version

no

### Python version

_No response_

---

_Label `bug` added by @kevinnudt12 on 2025-07-18 09:58_

---

_Comment by @konstin on 2025-07-18 10:07_

Can you share the complete commands you used and their complete output, in a code box?

---

_Comment by @kevinnudt12 on 2025-07-18 10:10_

Fixed! Thanks. curl worked. It is the network issue.

---

_Closed by @kevinnudt12 on 2025-07-18 10:10_

---

_Comment by @MohanaManikandan on 2025-07-23 05:34_

I am observing this issue with Cargo

```bash
cargo install --git https://github.com/astral-sh/uv uv
Updating git repository `https://github.com/astral-sh/uv`
error: could not find `uv` in https://github.com/astral-sh/uv with version `*`
```

**Operating System**: Debian GNU/Linux 12 (bookworm)  
**Kernel**: Linux 6.1.0-37-amd64
**Architecture**: x86-64


---

_Comment by @zanieb on 2025-07-23 13:09_

I can't reproduce that

---

_Comment by @MohanaManikandan on 2025-07-23 13:26_

Hi @zanieb. This is observed when the end user's Rust version is incompatible with uv's minimum supported Rust version. This was discussed and fixed in Issue #14835.

---

_Comment by @zanieb on 2025-07-23 13:28_

Thanks!

---
