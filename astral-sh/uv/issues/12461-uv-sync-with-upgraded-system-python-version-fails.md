```yaml
number: 12461
title: "`uv sync` with upgraded system Python version fails to update `activate_this.py`"
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2025-03-25T10:34:08Z
updated_at: 2025-04-15T10:01:16Z
url: https://github.com/astral-sh/uv/issues/12461
synced_at: 2026-01-12T16:01:03Z
```

# `uv sync` with upgraded system Python version fails to update `activate_this.py`

---

_@andersk_

### Summary

An uv virtual environment using the system Python version embeds a reference to the Python minor version number in `activate_this.py`.

```console
$ docker run --rm -it ubuntu:22.04
# apt update
# apt install -y curl python3 python3.11
# curl -LsSf https://astral.sh/uv/install.sh | sh
# source $HOME/.local/bin/env
# uv init --no-pin-python app
# cd app
# uv add numpy
# grep site-packages .venv/bin/activate_this.py 
for lib in "../lib/python3.10/site-packages".split(os.pathsep):
```

If we then upgrade the system’s Python version (simulated here by changing the `/usr/bin/python3` symlink, but an OS upgrade has the same effect), `uv sync` correctly installs wheels for the new Python version, but incorrectly leaves `activate_this.py` unchanged.

```console
# ln -nsf python3.11 /usr/bin/python3
# uv sync
# uv run python -c 'import numpy; print(numpy)'
<module 'numpy' from '/app/.venv/lib/python3.11/site-packages/numpy/__init__.py'>
# grep site-packages .venv/bin/activate_this.py
for lib in "../lib/python3.10/site-packages".split(os.pathsep):
```

There are ways to force it to fix `activate_this.py`, e.g. `uv sync -p python3.10; uv sync -p python3.11` (both commands are necessary). However, a simple `uv sync` ought to automatically update `activate_this.py` without further workarounds.

### Platform

Ubuntu 22.04 amd64

### Version

uv 0.6.9

### Python version

Python 3.10.12

---

_Label `bug` added by @andersk on 2025-03-25 10:34_

---

_Comment by @zanieb on 2025-04-01 18:31_

I think `uv sync` should discard the virtual environment entirely in this case, I'm very surprised it does not?

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-11 13:03_

---

_Closed by @jtfmumm on 2025-04-15 10:01_

---
