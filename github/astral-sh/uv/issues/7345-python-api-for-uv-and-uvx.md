---
number: 7345
title: "Python API for `uv` and `uvx`"
type: issue
state: closed
author: aaronsteers
labels:
  - question
assignees: []
created_at: 2024-09-12T23:20:04Z
updated_at: 2024-10-21T21:53:16Z
url: https://github.com/astral-sh/uv/issues/7345
synced_at: 2026-01-07T13:12:17-06:00
---

# Python API for `uv` and `uvx`

---

_Issue opened by @aaronsteers on 2024-09-12 23:20_

I would like to bundle `uv`/`uvx` in my Python app as a replacement for the [`venv`](https://docs.python.org/3/library/venv.html) standard library and `pip`-based installation methods - specifically in order to take advantage of faster install times.

Does `uv` have a Python interface besides the CLI, and is that stable(ish) enough to leverage in another Python project? I didn't see any mention of a Python API in the reference docs, but wanted to ask here just in case.

PS - thanks for the awesome products - I'm a huge fan of `uv` and `ruff` and I use them daily! 

---

_Renamed from "Python API for uv and uvx" to "Python API for `uv` and `uvx`" by @aaronsteers on 2024-09-12 23:20_

---

_Comment by @zanieb on 2024-09-13 01:36_

No we don't provide a Python API and we're pretty unlikely to anytime soon. The CLI is the official public interface and we want to focus on providing a great experience there rather than dividing our attention.

Glad you like the tools <3



---

_Label `question` added by @zanieb on 2024-09-13 01:36_

---

_Referenced in [airspeed-velocity/asv#1436](../../airspeed-velocity/asv/issues/1436.md) on 2024-10-07 00:35_

---

_Closed by @zanieb on 2024-10-21 21:53_

---

_Referenced in [arongergely/qgis-h3-toolkit-plugin#1](../../arongergely/qgis-h3-toolkit-plugin/issues/1.md) on 2025-02-05 10:58_

---

_Referenced in [pytorch/pytorch#160361](../../pytorch/pytorch/pulls/160361.md) on 2025-08-18 19:54_

---
