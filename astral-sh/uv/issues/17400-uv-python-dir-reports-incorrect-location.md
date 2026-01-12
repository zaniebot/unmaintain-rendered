```yaml
number: 17400
title: "`uv python dir` reports incorrect location"
type: issue
state: closed
author: berzi
labels:
  - question
assignees: []
created_at: 2026-01-10T09:04:53Z
updated_at: 2026-01-10T16:07:27Z
url: https://github.com/astral-sh/uv/issues/17400
synced_at: 2026-01-12T02:26:26Z
```

# `uv python dir` reports incorrect location

---

_Issue opened by @berzi on 2026-01-10 09:04_

### Summary

This might very well be me misunderstanding the command, but in that case I reckon at least documentation improvement would make sense.

`uv python dir` (with or without `--bin`) outputs a directory that does not exist which is different than what `uv run python -c "import sys; print(sys.executable)"` outputs.

In my case specifically:
```shell
# uv run python -c "import sys; print(sys.executable)"
/app/.venv/bin/python3
# uv python dir --bin
/root/.local/bin
# uv python dir
/root/.local/share/uv/python
```

This is run in a docker compose container with this environment:
```dockerfile
ENV UV_LINK_MODE=copy
ENV UV_NO_MANAGED_PYTHON=1
ENV UV_PYTHON_DOWNLOADS=never
ENV UV_CACHE_DIR=/app/.cache/uv
ENV VIRTUAL_ENV=/app/.venv
```
With uv installed this way in the Dockerfile:
```dockerfile
COPY --from=ghcr.io/astral-sh/uv:0.9.15 /uv /uvx /bin/
```

I was under the impression that the uv command would give me the location of the actual binary that runs when I run Python through uv, but that is evidently not the case here. If this is correct behaviour, then:
- Shouldn't this be clearer in the docs?
- What use case does the command have?

Or maybe it's actually a bug that the `UV_CACHE_DIR` is not taken into account?

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.9.15

### Python version

Python 3.14.1

---

_Label `bug` added by @berzi on 2026-01-10 09:04_

---

_Comment by @zanieb on 2026-01-10 11:11_

Please see `uv help python dir`?

---

_Label `bug` removed by @zanieb on 2026-01-10 11:11_

---

_Label `question` added by @zanieb on 2026-01-10 11:11_

---

_Comment by @berzi on 2026-01-10 11:18_

The help, just as the web docs, explain how the directory is selected, but in no way do they hint that the output may not actually exist or correspond to the true location of the binary.

---

_Comment by @zanieb on 2026-01-10 11:52_

Sorry I don't get the confusion? This is the "uv Python installation directory" where we store versions of Python. This is also described in our storage reference https://docs.astral.sh/uv/reference/storage/#python-versions and is consistent with other `dir` commands in our interface.

> I was under the impression that the uv command would give me the location of the actual binary that runs when I run Python through uv, but that is evidently not the case here.

It sounds like you want `uv python find`?

---

_Comment by @berzi on 2026-01-10 12:03_

‼️ Ok, my bad, I overlooked `uv python find` completely.

It took me a moment to understand why the shown dir would not exist (= because it only stores "global", i.e. non-venv, python versions installed through uv, which may not exist because uv runs things through the python in the venv).

---

_Closed by @berzi on 2026-01-10 12:03_

---

_Comment by @zanieb on 2026-01-10 16:07_

I can look at clarifying the top-level help here. I do appreciate the feedback!

---
