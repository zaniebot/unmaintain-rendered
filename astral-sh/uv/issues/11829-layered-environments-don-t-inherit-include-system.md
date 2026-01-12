```yaml
number: 11829
title: "Layered environments don't inherit `include-system-site-packages`"
type: issue
state: closed
author: zeevro
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-02-27T11:19:21Z
updated_at: 2025-03-01T03:22:38Z
url: https://github.com/astral-sh/uv/issues/11829
synced_at: 2026-01-12T16:00:47Z
```

# Layered environments don't inherit `include-system-site-packages`

---

_@zeevro_

### Summary

```
❯ uv --version
uv 0.6.3

❯ uv pip list
Using Python 3.13.2 environment at: /usr/local
Package Version
------- -------
pip     24.3.1

❯ uv pip install --system bottle
Using Python 3.13.2 environment at: /usr/local
Resolved 1 package in 378ms
Prepared 1 package in 208ms
Installed 1 package in 3ms
 + bottle==0.13.2

❯ uv venv --system-site-packages
Using CPython 3.13.2 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv

❯ uv run -m bottle --version
Bottle 0.13.2

❯ uv run --isolated -m bottle --version
/root/.cache/uv/builds-v0/.tmpTphYoC/bin/python: No module named bottle

❯ uv run --with peewee -m bottle --version
      Built peewee==3.17.9
Installed 1 package in 1ms
/root/.cache/uv/archive-v0/hAukBO_NFLtWh1qbW1e0U/bin/python3: No module named bottle
```

### Platform

Alpine Linux v3.21 docker on Ubuntu 24.04 x86-64

### Version

uv 0.6.3

### Python version

Python 3.13.2

---

_Label `bug` added by @zeevro on 2025-02-27 11:19_

---

_Comment by @charliermarsh on 2025-02-27 18:47_

👍 We'll need to decide if this is supported.

---

_Comment by @charliermarsh on 2025-02-28 02:52_

I suppose we should support it. We can use a pattern similar to `CachedEnvironment::set_overlay` and `CachedEnvironment::clear_overlay`, but for propagating that flag.

---

_Label `help wanted` added by @charliermarsh on 2025-02-28 02:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-01 02:31_

---

_Closed by @charliermarsh on 2025-03-01 03:22_

---
