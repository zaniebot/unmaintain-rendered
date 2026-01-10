---
number: 1547
title: uv pip install fails when using Nexus due to md5 hash
type: issue
state: closed
author: kendallbailey
labels:
  - compatibility
assignees: []
created_at: 2024-02-16T22:39:28Z
updated_at: 2024-02-17T00:25:18Z
url: https://github.com/astral-sh/uv/issues/1547
synced_at: 2026-01-10T01:23:07Z
---

# uv pip install fails when using Nexus due to md5 hash

---

_Issue opened by @kendallbailey on 2024-02-16 22:39_

Attempted to install a package through a Nexus pypi proxy.

```
$ uv pip install -i https://****:****@nexus.example.com/repository/pypi-group/simple gcsfs==2023.6.0
error: Received some unexpected HTML from https://****:****@nexus.example.com/repository/pypi-group/simple/gcsfs/
  Caused by: Unsupported hash algorithm (expected `sha256`) on: md5=02dfef373c7bd87df2121a92d82b2d5c
```

This doesn't happen with all packages installed via Nexus. Others such as `pandas` install without error. Tried some other google packages. `google-api-core==2.17.1` fails. `google-auth==2.28.0` works. Removing the `-i` option and letting `uv` hit pypi directly eliminates the errors.

---

_Comment by @charliermarsh on 2024-02-16 22:41_

Thanks, we can add md5.

---

_Label `compatibility` added by @charliermarsh on 2024-02-16 22:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 23:13_

---

_Referenced in [astral-sh/uv#1556](../../astral-sh/uv/pulls/1556.md) on 2024-02-17 00:13_

---

_Closed by @charliermarsh on 2024-02-17 00:25_

---

_Referenced in [astral-sh/uv#4887](../../astral-sh/uv/issues/4887.md) on 2024-11-07 02:41_

---
