---
number: 3556
title: "`uv run` should not require an existing virtual environment"
type: issue
state: closed
author: zanieb
labels:
  - preview
assignees: []
created_at: 2024-05-13T17:37:29Z
updated_at: 2024-05-13T19:04:26Z
url: https://github.com/astral-sh/uv/issues/3556
synced_at: 2026-01-10T01:23:29Z
---

# `uv run` should not require an existing virtual environment

---

_Issue opened by @zanieb on 2024-05-13 17:37_

Currently `uv run` fails if a virtual environment is not present

```
❯ uv run --with anyio -- python -c "print('hi')"
warning: `uv run` is experimental and may change without warning.
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.

```

We should not require a virtual environment to exist to run commands.

You can currently bypass this with `--isolated`

```
❯ uv --isolated run --with anyio -- python -c "print('hi')"
warning: `uv run` is experimental and may change without warning.
Resolved 3 packages in 195ms
Downloaded 3 packages in 78ms
Installed 3 packages in 8ms
 + anyio==4.3.0
 + idna==3.7
 + sniffio==1.3.1
hi
```


---

_Label `preview` added by @zanieb on 2024-05-13 17:38_

---

_Comment by @charliermarsh on 2024-05-13 18:11_

Are you sure this is true on `main`? It works fine for me.

---

_Comment by @charliermarsh on 2024-05-13 18:13_

I'm thinking of: https://github.com/astral-sh/uv/pull/3499

---

_Comment by @zanieb on 2024-05-13 19:04_

Ah I was running the latest release, presuming you fixed this then.

---

_Closed by @zanieb on 2024-05-13 19:04_

---
