---
number: 1752
title: venv --seed always downloads from pypi.org
type: issue
state: closed
author: oriori1703
labels:
  - bug
assignees: []
created_at: 2024-02-20T14:09:39Z
updated_at: 2024-02-21T06:24:47Z
url: https://github.com/astral-sh/uv/issues/1752
synced_at: 2026-01-10T01:23:08Z
---

# venv --seed always downloads from pypi.org

---

_Issue opened by @oriori1703 on 2024-02-20 14:09_

When trying to create a seeded venv, uv still tries to download the packages from pypi.org.
For example when blocking pypi.org in my dns and running `uv venv --seed --index-url https://mypypi.com/simple` the output is:
```
Creating virtualenv at: .venv
uv::venv::seed
Failed to install seed packages
|-> No solution found when resolving: pip, setuptools, wheel
|-> error sending request for url (https://pypi.org/simple/wheel/): error trying to connect: dns error: failed to lookup address information: Name or service not known
|-> error trying to connect: dns error: failed to lookup address information: Name or service not known
|-> dns error: failed to lookup address information: Name or service not known
`-> failed to lookup address information: Name or service not known
```

I have used:
uv version: `0.1.5`
OS: Ubuntu 20.04

---

_Label `bug` added by @charliermarsh on 2024-02-20 14:11_

---

_Comment by @charliermarsh on 2024-02-20 14:12_

Looks like a bug, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 14:38_

---

_Referenced in [astral-sh/uv#1755](../../astral-sh/uv/pulls/1755.md) on 2024-02-20 14:45_

---

_Closed by @charliermarsh on 2024-02-20 14:49_

---

_Comment by @oriori1703 on 2024-02-21 06:24_

Thanks for the quick fix 🙏

---
