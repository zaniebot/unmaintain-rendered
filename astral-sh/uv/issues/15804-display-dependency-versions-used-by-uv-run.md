---
number: 15804
title: "Display dependency versions used by \"uv run myscript.py\"?"
type: issue
state: closed
author: jondo
labels:
  - question
assignees: []
created_at: 2025-09-12T08:52:42Z
updated_at: 2025-09-12T09:06:49Z
url: https://github.com/astral-sh/uv/issues/15804
synced_at: 2026-01-10T01:26:00Z
---

# Display dependency versions used by "uv run myscript.py"?

---

_Issue opened by @jondo on 2025-09-12 08:52_

### Question

myscript.py has [inline dependencies](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) declared, and "uv run myscript.py" resolves and installs/uses them.

How can I see which actual versions of these dependencies and of secondary dependencies are used?

### Platform

Linux Mint 22.1

### Version

uv 0.8.17

---

_Label `question` added by @jondo on 2025-09-12 08:52_

---

_Renamed from "How can I see which dependency versions are used by "uv run myscript.py"?" to "Display dependency versions used by "uv run myscript.py"?" by @jondo on 2025-09-12 08:53_

---

_Comment by @jondo on 2025-09-12 09:06_

Ah, the answer is `uv tree --script myscript.py`!
(Or, to keep the versions locked: `uv lock --script myscript.py`.)

---

_Closed by @jondo on 2025-09-12 09:06_

---
