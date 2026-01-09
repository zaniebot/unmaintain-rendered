---
number: 13912
title: "INP001: Skip files with PEP 723 inline script metadata"
type: issue
state: closed
author: konstin
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-10-24T14:21:46Z
updated_at: 2024-10-29T02:11:32Z
url: https://github.com/astral-sh/ruff/issues/13912
synced_at: 2026-01-07T13:12:16-06:00
---

# INP001: Skip files with PEP 723 inline script metadata

---

_Issue opened by @konstin on 2024-10-24 14:21_

I have a single Python file in a directory with a PEP 723 inline script header at the top of the file:

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "httpx>=0.27.2,<0.28",
#     "packaging>=24.1,<25",
# ]
# ///
```

Similar to #8690, `INP001` should skip such files: They are not meant to be imported, but meant to be run as standalone scripts with `uv run folder/my_script.py` or `pipx run folder/my_script.py`.

---

_Label `rule` added by @MichaReiser on 2024-10-24 14:27_

---

_Label `help wanted` added by @MichaReiser on 2024-10-24 14:27_

---

_Comment by @charliermarsh on 2024-10-24 14:47_

Good call.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-29 01:35_

---

_Referenced in [astral-sh/ruff#13974](../../astral-sh/ruff/pulls/13974.md) on 2024-10-29 02:05_

---

_Closed by @charliermarsh on 2024-10-29 02:11_

---
