---
number: 6550
title: uv run --no-project fails due to missing pyproject.toml sections
type: issue
state: closed
author: meastham
labels:
  - bug
assignees: []
created_at: 2024-08-23T22:15:15Z
updated_at: 2024-08-23T23:04:40Z
url: https://github.com/astral-sh/uv/issues/6550
synced_at: 2026-01-10T01:24:02Z
---

# uv run --no-project fails due to missing pyproject.toml sections

---

_Issue opened by @meastham on 2024-08-23 22:15_

Similar issue to https://github.com/astral-sh/uv/issues/6177

If I run `uv run --no-project ...` in a directory with a `pyproject.toml` with a missing `project` table, I get an error:

```
❯ uv -V
uv 0.3.2 (c5440001c 2024-08-23)
❯ cd $(mktemp -d)
❯ touch pyproject.toml
❯ uv run --no-project python
error: No `project` table found in: `/private/var/folders/1s/0jgtfqgd1wqc327r1kxgrh3w0000gn/T/tmp.mAGDf4eLya/pyproject.toml`
```

Same result with `--no-config`

(this is on MacOS / apple silicon)

---

_Label `bug` added by @zanieb on 2024-08-23 22:18_

---

_Comment by @charliermarsh on 2024-08-23 22:18_

Thanks, my bad, I'll fix that now.

---

_Referenced in [astral-sh/uv#6552](../../astral-sh/uv/pulls/6552.md) on 2024-08-23 22:21_

---

_Referenced in [astral-sh/uv#6554](../../astral-sh/uv/pulls/6554.md) on 2024-08-23 22:27_

---

_Closed by @zanieb on 2024-08-23 23:04_

---

_Closed by @zanieb on 2024-08-23 23:04_

---
