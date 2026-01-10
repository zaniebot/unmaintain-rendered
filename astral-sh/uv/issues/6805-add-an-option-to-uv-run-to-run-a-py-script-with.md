---
number: 6805
title: "Add an option to `uv run` to run a `.py` script with `pythonw.exe` instead of `python.exe`"
type: issue
state: closed
author: piepero
labels:
  - enhancement
  - good first issue
  - cli
assignees: []
created_at: 2024-08-29T11:47:23Z
updated_at: 2024-12-31T17:19:44Z
url: https://github.com/astral-sh/uv/issues/6805
synced_at: 2026-01-10T01:24:06Z
---

# Add an option to `uv run` to run a `.py` script with `pythonw.exe` instead of `python.exe`

---

_Issue opened by @piepero on 2024-08-29 11:47_

I am using `uv run` for a simple Python script with inline dependencies.

This script is used in two ways/environments.

First, it is run from the console. In this case, it should be run using `python.exe`, thus showing stout/stderr on the console.

Second, the same script is run by the Windows 11 Task Scheduler. In this case, I would like to use the windowless `pythonw.exe` to run the script without opening another window.

Currently, the only way to get `uv run` to choose either one or the other interpreter, is by naming the script either `myscript.py` or `myscript.pyw`.

In other words, I have to maintain two otherwise identical copies of the same script.

There may be workarounds, but the ideas I came up with either did not work (`uv run pythonw myscript.py` does not parse the inline metadata) or were cumbersome and error prone (i.e. using Windows soft or hard links, or importing the `.py` script from a second `.pyw` script and keeping the inline dependencies in sync).

It would be very convenient, if there was an additional flag for `uv run` to instruct `uv` to use `pythonw.exe` on a file with the `.py` extension (e.g. `uv run --pythonw myscript.py`).

---

_Assigned to @zanieb by @zanieb on 2024-08-29 13:43_

---

_Label `cli` added by @zanieb on 2024-08-29 13:43_

---

_Label `enhancement` added by @zanieb on 2024-08-29 13:43_

---

_Comment by @zanieb on 2024-08-29 13:44_

We could parse the metadata with `pythonw myscript.py`, I think? Alternatively, we can provide a `--script-exec` option that we use to execute scripts per https://github.com/astral-sh/uv/issues/6542#issuecomment-2307709992

---

_Comment by @zanieb on 2024-10-04 21:49_

We added a `--script` flag in #7739 but we don't have a `--gui-script` flag — which would be Windows-only but reasonable to include, I think.

---

_Label `good first issue` added by @zanieb on 2024-10-04 21:49_

---

_Unassigned @zanieb by @zanieb on 2024-10-04 21:49_

---

_Referenced in [astral-sh/uv#9152](../../astral-sh/uv/pulls/9152.md) on 2024-11-15 14:58_

---

_Comment by @Choudhry18 on 2024-12-31 09:00_

@zanieb I just got interested in the project and was looking at good first issues to start, this issue is still open but after reading the pull request #9152 it seems the issue should be closed.

---

_Comment by @zanieb on 2024-12-31 17:19_

Yep this is implemented, sorry!

---

_Closed by @zanieb on 2024-12-31 17:19_

---
