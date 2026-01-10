---
number: 13107
title: UV_SYSTEM_PYTHON and UV_PROJECT_ENVIRONMENT not work for not privileged users in docker (pip install --user mode)
type: issue
state: open
author: moaddib666
labels:
  - question
assignees: []
created_at: 2025-04-25T12:49:30Z
updated_at: 2025-04-29T08:05:51Z
url: https://github.com/astral-sh/uv/issues/13107
synced_at: 2026-01-10T01:25:29Z
---

# UV_SYSTEM_PYTHON and UV_PROJECT_ENVIRONMENT not work for not privileged users in docker (pip install --user mode)

---

_Issue opened by @moaddib666 on 2025-04-25 12:49_

### Summary

I'm currently trying to migrate my Docker-based Python setup from pipenv/pip to uv, but I'm running into issues with package installation.

With pip, when system-level Python is not writable, it automatically falls back to --user installs, placing packages in ~/.local/. Python can still detect and use these packages when run as a user.

However, uv doesn’t seem to support this fallback. I haven’t found any option to force uv to install packages in user space (~/.local) when system paths are not writable.

As a result, I either get:

Permission denied (os error 13) when trying to install to /usr/local/lib/python3.12/site-packages/

Or errors like:
"~/.local/lib/..." is not a valid Python environment (no Python executable found)

This makes it difficult to use uv in non-root Docker setups, where I can’t write to system directories.

### Platform

Alpine, Ubuntu (docker)

### Version

0.6.16

### Python version

Python 3.12.10

---

_Label `bug` added by @moaddib666 on 2025-04-25 12:49_

---

_Comment by @charliermarsh on 2025-04-26 14:03_

Unfortunately we don't support `--user` installs -- that's intentional. If you want to use uv to install into a system in which you don't have write access (even if it's in Docker), you should use a virtual environment. Here are some resources that might be helpful for you:

- https://hynek.me/articles/docker-virtualenv/
- https://hynek.me/articles/docker-uv/

---

_Label `bug` removed by @charliermarsh on 2025-04-26 14:03_

---

_Label `question` added by @charliermarsh on 2025-04-26 14:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-26 14:03_

---

_Comment by @moaddib666 on 2025-04-29 08:05_

Hi @charliermarsh —

I totally get the “just use venv” approach, but in Docker we often run one immutable image that act as API or worker or cron depends on the command. 

Adding a venv :

- duplicates the Python runtime inside the image

- forces us to tweak PATH/PYTHONPATH 

- makes on-container debugging harder (everything lives under venv/… instead of the standard tree)

pip’s fallback to ~/.local solves this neatly: the image stays read-only, each UID inside the container can still pip-install tricks for its process, and nothing else needs to change.

That would let us adopt `uv` in existing images without a full rebuild around virtual-env layouts.

Thanks for the pointers & for pushing the tooling forward!

---
