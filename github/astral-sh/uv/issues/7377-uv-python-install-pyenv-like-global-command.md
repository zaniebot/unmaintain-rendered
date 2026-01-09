---
number: 7377
title: "uv python install: pyenv-like \"global\" command?"
type: issue
state: closed
author: arwhyte
labels:
  - duplicate
assignees: []
created_at: 2024-09-13T19:49:15Z
updated_at: 2024-09-14T01:01:09Z
url: https://github.com/astral-sh/uv/issues/7377
synced_at: 2026-01-07T13:12:17-06:00
---

# uv python install: pyenv-like "global" command?

---

_Issue opened by @arwhyte on 2024-09-13 19:49_

Migrating to `uv`. But I have a question that I've not been able resolve via the documentation.

`pyenv` provides a global command for establishing the preferred Python version that will be run instead of the system Python when using the terminal, etc.

```commandline
pyenv install 3.12.5
pyenv global 3.12.5
```

Does `uv` provide a similar command or a way to set the default Python so that my terminal recognizes my `uv` install of 3.12.5?


---

_Comment by @charliermarsh on 2024-09-14 00:30_

Thanks -- I believe this a duplicate of #6265.

---

_Closed by @charliermarsh on 2024-09-14 00:30_

---

_Label `duplicate` added by @charliermarsh on 2024-09-14 00:30_

---

_Comment by @charliermarsh on 2024-09-14 00:30_

(We do plan to provide this but it doesn't exist yet.)

---

_Comment by @arwhyte on 2024-09-14 01:00_

@charliermarsh thanks for the quick reply. Enjoyed listening to you on https://talkpython.fm/episodes/show/476/unified-python-packaging-with-uv

---

_Comment by @charliermarsh on 2024-09-14 01:01_

Thanks, I appreciate that :)

---
