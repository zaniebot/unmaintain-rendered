---
number: 11779
title: uv run uses older version of dependency even after upgrading
type: issue
state: closed
author: pchalasani
labels:
  - question
assignees: []
created_at: 2025-02-25T16:26:05Z
updated_at: 2025-02-25T17:02:49Z
url: https://github.com/astral-sh/uv/issues/11779
synced_at: 2026-01-07T13:12:18-06:00
---

# uv run uses older version of dependency even after upgrading

---

_Issue opened by @pchalasani on 2025-02-25 16:26_

### Question

I'm seeing some odd behavior and not sure if it's a bug or my ignorance:

Say my project uses some lib `blah >= 0.1.3` as a dependency in `pyproject.toml`

Then I upgrade all dependencies using the following 
(which by the way I found after _a lot of trial and error_ since it was not clear in the docs how
to use uv to upgrade all dependencies to their latest available versions consistent with the 
constraints in `pyproject.toml`):

```bash
uv pip install -r pyproject.toml --upgrade
```

This updates `blah` to 0.1.6, so when I run my script:

```bash
uv run scripts/main.py
```

I expect it to use `blah 0.1.6`, but this `uv run` re-installs the **older** version of blah, like 0.1.3, before running the script.

I know I can simply do `python run scripts/main.py` -- but I thought `uv run` would do the same.


### Platform

MacOS arm

### Version

_No response_

---

_Label `question` added by @pchalasani on 2025-02-25 16:26_

---

_Comment by @charliermarsh on 2025-02-25 16:39_

You probably don't want to be using `uv pip install` alongside `uv run`. `uv pip install` won't affect the lockfile, so `uv run` will then re-sync your project with the locked constraints. (You generally want to use the `uv pip` APIs _or_ `uv run` / `uv lock` / `uv sync`, but not both together.) I'd suggest `uv sync --upgrade` instead of `uv pip install -r pyproject.toml --upgrade`.

---

_Comment by @pchalasani on 2025-02-25 17:02_

> I'd suggest `uv sync --upgrade` instead of `uv pip install -r pyproject.toml --upgrade`.

Ah I see where I went wrong, thanks for clearing that up!

---

_Closed by @pchalasani on 2025-02-25 17:02_

---
