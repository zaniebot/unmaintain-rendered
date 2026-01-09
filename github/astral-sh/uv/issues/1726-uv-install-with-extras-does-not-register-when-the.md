---
number: 1726
title: uv install with extras does not register when the package is already installed
type: issue
state: closed
author: hsheth2
labels:
  - bug
assignees: []
created_at: 2024-02-20T01:28:43Z
updated_at: 2024-02-20T03:37:37Z
url: https://github.com/astral-sh/uv/issues/1726
synced_at: 2026-01-07T13:12:16-06:00
---

# uv install with extras does not register when the package is already installed

---

_Issue opened by @hsheth2 on 2024-02-20 01:28_

Doing a `install package` followed by `install package[extra]` does not install extras.

```sh
uv venv
source .venv/bin/activate

uv pip install 'acryl-datahub'
uv pip install 'acryl-datahub[snowflake]'  # ❌ this does not install anything new, even though it should

# Uninstalling first and then doing an install works though:
uv pip uninstall acryl-datahub && uv pip install 'acryl-datahub[snowflake]'  # ✅ 
```

A similar series of commands works as expected with pip.

```sh
$ uv --version
uv 0.1.5
$ uname -mo
Darwin x86_64
```


---

_Referenced in [datahub-project/datahub#9885](../../datahub-project/datahub/pulls/9885.md) on 2024-02-20 01:29_

---

_Label `bug` added by @charliermarsh on 2024-02-20 01:29_

---

_Comment by @charliermarsh on 2024-02-20 01:29_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 01:29_

---

_Referenced in [astral-sh/uv#1727](../../astral-sh/uv/pulls/1727.md) on 2024-02-20 01:49_

---

_Closed by @charliermarsh on 2024-02-20 03:37_

---
