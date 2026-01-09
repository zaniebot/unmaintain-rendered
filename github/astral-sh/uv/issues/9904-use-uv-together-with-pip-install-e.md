---
number: 9904
title: use uv together with pip install -e
type: issue
state: closed
author: ghost
labels:
  - question
assignees: []
created_at: 2024-12-15T06:06:37Z
updated_at: 2024-12-15T14:22:21Z
url: https://github.com/astral-sh/uv/issues/9904
synced_at: 2026-01-07T13:12:18-06:00
---

# use uv together with pip install -e

---

_Issue opened by @ghost on 2024-12-15 06:06_

is it possible to use uv together with pip install -e

---

_Comment by @neutrinoceros on 2024-12-15 07:34_

Yes. A couple notes:
- `pip` (or pip-tools) isn't available by default in an env created via `uv venv`, but you can get it via `uv pip install pip`
- `uv pip tree` includes packages installed via regular `pip`
- if you meant to ask if `uv pip install` supported the `-e/--editable` flag, the answer is also yes

I don't personally have a use for `pip` since switching to `uv`, but the few ways I can *think* of combining both appear to work seamlessly.

---

_Label `question` added by @charliermarsh on 2024-12-15 13:46_

---

_Closed by @ghost on 2024-12-15 14:22_

---
