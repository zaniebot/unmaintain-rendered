---
number: 10635
title: "Indicate platform or Python version mismatch in `uv sync` errors"
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2025-01-15T16:41:14Z
updated_at: 2025-01-20T17:46:47Z
url: https://github.com/astral-sh/uv/issues/10635
synced_at: 2026-01-10T01:24:56Z
---

# Indicate platform or Python version mismatch in `uv sync` errors

---

_Issue opened by @charliermarsh on 2025-01-15 16:41_

E.g.:

```
error: Distribution tensorflow-io-gcs-filesystem==0.37.1 @ registry+https://pypi.org/simple can't be installed because it doesn't have a source distribution or wheel for the current platform`
```

As in the resolver, we should tell the user what the mismatch is. Are there wheels for the current Python version, but not the platform? The other way around?

---

_Label `error messages` added by @charliermarsh on 2025-01-15 16:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-15 16:45_

---

_Comment by @charliermarsh on 2025-01-18 16:27_

I'm going to do this, it's bothering me a ton and we get lots of reports about it. Maybe our worst error message experience!

---

_Referenced in [astral-sh/uv#10739](../../astral-sh/uv/pulls/10739.md) on 2025-01-18 19:16_

---

_Closed by @charliermarsh on 2025-01-20 17:46_

---

_Closed by @charliermarsh on 2025-01-20 17:46_

---
