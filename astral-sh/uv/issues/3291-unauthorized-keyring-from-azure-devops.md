```yaml
number: 3291
title: Unauthorized keyring from Azure DevOps
type: issue
state: closed
author: AndreuCodina
labels:
  - compatibility
assignees: []
created_at: 2024-04-28T09:57:49Z
updated_at: 2024-04-28T14:06:44Z
url: https://github.com/astral-sh/uv/issues/3291
synced_at: 2026-01-10T05:31:37Z
```

# Unauthorized keyring from Azure DevOps

---

_Issue opened by @AndreuCodina on 2024-04-28 09:57_

When I use the keyring authentication and try to install packages from a private registry in Azure DevOps, I receive `401 Unauthorized`.
I log in with my Microsoft account with `az login`.
The installation currently works with `pip` instead of `uv`.

**requirements.txt**
```
sqlmodel==0.0.14
my-private-package==1.0.0
```

**Execution:**

```bash
$ uv --version
uv 0.1.39
```

```bash
$ uv pip install --system keyring artifacts-keyring
...

$ az login
...

$ uv pip install --system --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --keyring-provider subprocess --requirement requirements.txt --verbose

DEBUG Starting interpreter discovery for default Python
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /usr/local/bin/python3
DEBUG Using Python 3.12.3 environment at /usr/local/bin/python3
DEBUG Trying to lock if free: /tmp/uv-39ee7c309557c5e9.lock
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: sqlmodel==0.0.14
DEBUG Adding direct dependency: my-private-package==1.0.0
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
error: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/)
```

I've tried to generate a token and executing:

--extra-index-url=https:// **mytokenname@** pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple

But I get the same result.

---

_Comment by @charliermarsh on 2024-04-28 12:54_

Hmm, strange. They return a 401 for a package that doesn't exist?

---

_Comment by @charliermarsh on 2024-04-28 12:54_

Are you able to try running from this branch? https://github.com/astral-sh/uv/pull/3292

---

_Label `compatibility` added by @charliermarsh on 2024-04-28 12:54_

---

_Comment by @AndreuCodina on 2024-04-28 13:44_

> Are you able to try running from this branch? #3292

How can I test it from that branch? I install uv with `pip install uv`.

---

_Closed by @charliermarsh on 2024-04-28 14:06_

---
