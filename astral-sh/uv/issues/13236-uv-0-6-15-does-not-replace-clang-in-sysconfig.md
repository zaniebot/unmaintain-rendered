```yaml
number: 13236
title: "`uv>=0.6.15` does not replace `clang` in `sysconfig` during installation."
type: issue
state: closed
author: sabonerune
labels:
  - bug
assignees: []
created_at: 2025-04-30T16:47:34Z
updated_at: 2025-04-30T17:23:23Z
url: https://github.com/astral-sh/uv/issues/13236
synced_at: 2026-01-10T03:41:47Z
```

# `uv>=0.6.15` does not replace `clang` in `sysconfig` during installation.

---

_Issue opened by @sabonerune on 2025-04-30 16:47_

### Summary

#8429
It seems the problem has reappeared in v0.6.15.

```bash
curl -LsSf https://astral.sh/uv/0.16.14/install.sh | sh
uv python install --managed-python 3.13
uv run --python 3.13 --managed-python -m sysconfig | grep "CC ="
#         CC = "cc -pthread"
#         LINKCC = "cc -pthread -fno-pie -no-pie"
uv run --python 3.13 --managed-python -m sysconfig | grep "CXX ="
#         CXX = "c++ -pthread"
uv self update 0.6.15
uv python install --reinstall --managed-python 3.13
uv run --python 3.13 --managed-python -m sysconfig | grep "CC ="
#         CC = "clang -pthread"
#         LINKCC = "cc -pthread -fno-pie -no-pie"
uv run --python 3.13 --managed-python -m sysconfig | grep "CXX ="
#         CXX = "clang++ -pthread"
```

This issue still exists in at least 0.7.1.

### Platform

Ubuntu 20.04

### Version

0.6.15

### Python version

_No response_

---

_Label `bug` added by @sabonerune on 2025-04-30 16:47_

---

_Comment by @zanieb on 2025-04-30 16:54_

Thanks!

---

_Comment by @zanieb on 2025-04-30 16:55_

I can only presume this regressed in https://github.com/astral-sh/uv/pull/12239

---

_Closed by @zanieb on 2025-04-30 17:23_

---

_Closed by @zanieb on 2025-04-30 17:23_

---
