---
number: 9847
title: "UX: hard to tell which python uv will use"
type: issue
state: closed
author: hwine
labels: []
assignees: []
created_at: 2024-12-12T17:57:50Z
updated_at: 2024-12-13T01:25:24Z
url: https://github.com/astral-sh/uv/issues/9847
synced_at: 2026-01-10T01:24:46Z
---

# UX: hard to tell which python uv will use

---

_Issue opened by @hwine on 2024-12-12 17:57_

TBH, I'm not sure if this is a bug, or a subtle part of the python selection process, but I'm never sure _which_ python `uv` will use.

For example:
```
❯ uv --python-preference=only-system python list
cpython-3.13.0-windows-aarch64-none    C:\Users\hwine\AppData\Local\Programs\Python\Python313-arm64\python.exe
cpython-3.12.2-windows-aarch64-none    C:\Users\hwine\.pyenv\pyenv-win\versions\3.12.2-arm\python.exe
```
leads me to believe that version 3.13.0 will be used, but 3.12.2 is used:
```
❯ uv --python-preference=only-system run python
Python 3.12.2 (tags/v3.12.2:6abddd9, Feb  6 2024, 22:02:46) [MSC v.1937 64 bit (ARM64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
```

I would like some way to see in advance which python `uv` will run for a given `python-preference`.

---

_Referenced in [astral-sh/uv#9841](../../astral-sh/uv/pulls/9841.md) on 2024-12-12 18:00_

---

_Comment by @FishAlchemist on 2024-12-12 18:22_

``uv run`` is a project-level API that seems to prioritize the project's metadata, specifically the 'requires-python' and '.python-version' files.
Furthermore, uv typically doesn't change the Python version unless the current one doesn't align with the specified requires-python and .python-version.

---

_Comment by @zanieb on 2024-12-12 18:35_

I think you're just looking for `uv python find` which will tell you which interpreter we'd use.

---

_Comment by @zanieb on 2024-12-12 18:36_

(`uv python list` is not sorted by priority, but by version)

---

_Comment by @hwine on 2024-12-13 01:25_

> I think you're just looking for `uv python find` which will tell you which interpreter we'd use.

\o/ That is, indeed, exactly what I was looking for.

---

_Closed by @hwine on 2024-12-13 01:25_

---
