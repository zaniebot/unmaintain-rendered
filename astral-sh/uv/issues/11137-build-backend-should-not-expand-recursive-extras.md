---
number: 11137
title: Build backend should not expand recursive extras
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2025-01-31T18:01:19Z
updated_at: 2025-02-01T01:58:52Z
url: https://github.com/astral-sh/uv/issues/11137
synced_at: 2026-01-10T01:25:01Z
---

# Build backend should not expand recursive extras

---

_Issue opened by @charliermarsh on 2025-01-31 18:01_

I asked about this [here](https://discuss.python.org/t/core-metadata-for-self-referential-extras/77793). There wasn't consensus either way, but most people suggested it was fine for a backend to expand these (or not). I think we should _not_ expand them, since it doesn't match the metadata you get from reading the `pyproject.toml` directly and it's actually sort of a lossy transform (you can't identify a self-referential extra afterwards).

---

_Label `preview` added by @charliermarsh on 2025-01-31 18:01_

---

_Label `bug` added by @charliermarsh on 2025-01-31 18:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-31 19:24_

---

_Referenced in [astral-sh/uv#11142](../../astral-sh/uv/pulls/11142.md) on 2025-01-31 19:26_

---

_Closed by @charliermarsh on 2025-02-01 01:58_

---

_Closed by @charliermarsh on 2025-02-01 01:58_

---
