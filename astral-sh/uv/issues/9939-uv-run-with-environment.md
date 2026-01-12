```yaml
number: 9939
title: "`uv run` with environment?"
type: issue
state: closed
author: ameya98
labels: []
assignees: []
created_at: 2024-12-16T15:59:12Z
updated_at: 2024-12-16T16:29:12Z
url: https://github.com/astral-sh/uv/issues/9939
synced_at: 2026-01-12T16:00:02Z
```

# `uv run` with environment?

---

_@ameya98_

I created two environments `.venv` and `.venv-analysis` in the same folder.
I activated `.venv-analysis` but when I try running this:
```bash
$ uv run -m pip install wheel pyemma
warning: `VIRTUAL_ENV=.venv-analysis` does not match the project environment path `.venv` and will be ignored
...
```
it seems like `uv` still runs the environment under `.venv`? I would like `uv run` to use the currently activated environment. 

---

_Comment by @zanieb on 2024-12-16 16:01_

I think you want `uv run --no-project ...`

---

_Comment by @ameya98 on 2024-12-16 16:29_

Thank you! This was a bit confusing for me, because I didn't understand the single project <-> single virtualenv philosophy that `uv` has.

---

_Closed by @ameya98 on 2024-12-16 16:29_

---
