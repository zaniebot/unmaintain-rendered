---
number: 7637
title: "`uv python install '>=3.11'` selects pre-release version"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2024-09-23T13:26:33Z
updated_at: 2024-09-23T19:09:01Z
url: https://github.com/astral-sh/uv/issues/7637
synced_at: 2026-01-10T01:24:17Z
---

# `uv python install '>=3.11'` selects pre-release version

---

_Issue opened by @zanieb on 2024-09-23 13:26_

I have a similar issue with the usage of the following type of command:

```bash
uv python install '>=3.11'
```
With the latest pre-release Python version 3.13.0rc2 being available, if no other version is installed, uv will install 3.13.0rc2.

Would it not be better if uv installed the latest stable version (3.12.6) in that scenario?

As explained in [PEP440](https://peps.python.org/pep-0440/#version-specifiers), section **Handling of pre-release**,
"Pre-releases of any kind, including developmental releases, are implicitly excluded from all version specifiers, unless they are already present on the system, explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release."

Also, I am new to creating issues or requests, so let me know if I am doing something wrong.

_Originally posted by @Seazs in https://github.com/astral-sh/uv/issues/7429#issuecomment-2368096967_
            

---

_Referenced in [astral-sh/uv#7429](../../astral-sh/uv/issues/7429.md) on 2024-09-23 13:26_

---

_Referenced in [astral-sh/uv#7638](../../astral-sh/uv/pulls/7638.md) on 2024-09-23 13:36_

---

_Closed by @zanieb on 2024-09-23 19:09_

---

_Closed by @zanieb on 2024-09-23 19:09_

---
