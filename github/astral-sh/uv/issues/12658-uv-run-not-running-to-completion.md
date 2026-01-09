---
number: 12658
title: "`uv run` not running to completion"
type: issue
state: open
author: amotl
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-04-03T19:10:18Z
updated_at: 2025-05-09T14:49:19Z
url: https://github.com/astral-sh/uv/issues/12658
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv run` not running to completion

---

_Issue opened by @amotl on 2025-04-03 19:10_

### Summary

Hi. uv 0.6.12 user here. We wanted to report a problem that trips us when trying to invoke `uv run` from a Python process, in this case using the [mcp library](https://pypi.org/project/mcp/), where the program does not terminate (i.e. stalls), while invoking the same program using `python` works well.

We added a gist including the reproducer program [`uv_run_stuck_mcp.py`](https://gist.github.com/amotl/a761566e69870520c556f32c889ebeab), which you can use to replicate the flaw when invoking it like this:
```
python uv_run_stuck_mcp.py --uv
```

We've identified those issues to be possibly related to this problem, but we are not sure about it.
- GH-3095
- GH-11009
- GH-11886
- GH-12108


### Platform

macOS

### Version

uv 0.6.12 (Homebrew 2025-04-02)

### Python version

Python 3.13.2

---

_Label `bug` added by @amotl on 2025-04-03 19:10_

---

_Comment by @zanieb on 2025-04-03 21:23_

Thanks for the reproduction and issue links. Do you think you could minimize the reproduction further? I don't think I'll be able to debug it easily as-is.

---

_Comment by @amotl on 2025-04-08 20:57_

Hi Zanie. I absolutely hear you that it's not a good reproducer, but I am currently not able to come up with a trimmed-down version. It must be revolving around "doing something with Python `asyncio` plus starting a process in the background", like MCP servers are doing it, or something like this.


---

_Label `needs-mre` added by @zanieb on 2025-05-09 14:49_

---
