```yaml
number: 12298
title: "Please support (and respect) --link-mode in `uv python install`"
type: issue
state: closed
author: nbelakovski
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-03-18T19:04:06Z
updated_at: 2025-04-10T03:52:34Z
url: https://github.com/astral-sh/uv/issues/12298
synced_at: 2026-01-10T03:41:47Z
```

# Please support (and respect) --link-mode in `uv python install`

---

_Issue opened by @nbelakovski on 2025-03-18 19:04_

### Summary

A number of `uv` commands support the --link-mode option, but not `python install`. As a result when I mount a directory with a venv inside docker for local development, it can't python python because it's symlinked to a directory that's not in the container.

It seems like it should be straightforward to add this to python install? And of course if I run `uv sync --link-mode <option>` then the python it chooses to install or use should respect that as well.

Does this make sense and is it a reasonable request?

### Example

_No response_

---

_Label `enhancement` added by @nbelakovski on 2025-03-18 19:04_

---

_Comment by @zanieb on 2025-03-18 19:13_

To pick at the details a bit here: this isn't a `python install` behavior, it's virtual environment behavior. Copying the Python installation into virtual environments makes them "full" environments — not virtual. We'd need a larger concept than `link-mode` to represent this since we wouldn't actually be creating virtual environments anymore.

You can track support for this in https://github.com/astral-sh/uv/issues/7865

---

_Label `duplicate` added by @zanieb on 2025-03-18 19:13_

---

_Closed by @zanieb on 2025-04-10 03:52_

---
