---
number: 14623
title: uv pip install -r requirements.txt does not update toml file
type: issue
state: closed
author: jugeshPrajapati
labels:
  - question
assignees: []
created_at: 2025-07-15T10:29:59Z
updated_at: 2025-07-16T13:42:40Z
url: https://github.com/astral-sh/uv/issues/14623
synced_at: 2026-01-10T01:25:46Z
---

# uv pip install -r requirements.txt does not update toml file

---

_Issue opened by @jugeshPrajapati on 2025-07-15 10:29_

### Summary

I created a new project in Windows and I installed some packages using "uv pip install -r requirements.txt" in .venv, but it can not update the toml file automatically, if it can't update or create a new one from the updated .venv, so I can't reproduce the same project using the toml file. And those .toml and lock files are useless.

### Example

_No response_

---

_Label `enhancement` added by @jugeshPrajapati on 2025-07-15 10:30_

---

_Comment by @FishAlchemist on 2025-07-15 12:23_

The `uv pip` series of commands inherently doesn't update `pyproject.toml`. After all, that part is merely a reimplementation (not fully compatible) of `pip`'s commands and some of its ecosystem functionalities.

 `uv`'s Project API series will modify `pyproject.toml` and `uv.lock`.
If your `requirements.txt` doesn't contain indirect dependencies, you can just use `uv add -r requirements.txt`
https://docs.astral.sh/uv/concepts/projects/dependencies/#importing-dependencies

---

_Label `enhancement` removed by @zanieb on 2025-07-15 12:29_

---

_Label `question` added by @zanieb on 2025-07-15 12:29_

---

_Comment by @charliermarsh on 2025-07-15 18:55_

Yeah, I think you want `uv add -r requirements.txt`.

---

_Closed by @charliermarsh on 2025-07-16 13:42_

---
