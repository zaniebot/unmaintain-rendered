---
number: 10004
title: Support .netrc files for authenatication
type: issue
state: closed
author: janbernloehr
labels: []
assignees: []
created_at: 2024-02-16T00:47:22Z
updated_at: 2024-02-16T01:11:54Z
url: https://github.com/astral-sh/ruff/issues/10004
synced_at: 2026-01-07T13:12:15-06:00
---

# Support .netrc files for authenatication

---

_Issue opened by @janbernloehr on 2024-02-16 00:47_

pip supports obtaining credentials for authentication from `.netrc` files. This allows to keep `pip.conf` free of credentials and instead store them in a central place in `~/.netrc`. Unfortunately, `uv` does not support obtaining credentials from there.


---

_Closed by @janbernloehr on 2024-02-16 00:58_

---

_Comment by @charliermarsh on 2024-02-16 01:11_

Was wondering why you closed this, then realized the repo :joy:

---
