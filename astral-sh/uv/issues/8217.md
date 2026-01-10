```yaml
number: 8217
title: Notify users when a new Python patch version is available
type: issue
state: open
author: itamarst
labels:
  - enhancement
assignees: []
created_at: 2024-10-15T13:44:32Z
updated_at: 2025-01-14T20:49:32Z
url: https://github.com/astral-sh/uv/issues/8217
synced_at: 2026-01-10T04:27:58Z
```

# Notify users when a new Python patch version is available

---

_Issue opened by @itamarst on 2024-10-15 13:44_

I'm running on 3.12.7. CPython releases 3.12.8 with security fixes. It would be good for `uv` to notify users that they want to install this new version.

Compare to Linux distribution behavior, where there's an auto-update mechanism that might (depending on distro) pop up saying "updates available, click here to install".

(See also https://github.com/astral-sh/uv/issues/1795#issuecomment-2413939107 for some motivation re making existing venvs secure.)

---

_Comment by @zanieb on 2024-10-15 14:02_

Good idea :)

---

_Label `enhancement` added by @zanieb on 2024-10-15 14:02_

---

_Comment by @s-banach on 2024-11-04 20:47_

Sorry to bother you both, hopefully this question is on-topic.
Is it possible to update the python version in an existing venv without destroying the venv?
Thanks.

---

_Comment by @zanieb on 2024-11-04 21:01_

Only to a different patch version of the same minor version — and it's sort of a workaround e.g.

```
❯ uv venv -p 3.11
Using CPython 3.11.10
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install anyio
Resolved 3 packages in 118ms
Installed 3 packages in 5ms
 + anyio==4.6.2.post1
 + idna==3.10
 + sniffio==1.3.1
❯ ls .venv/lib/python3.11/site-packages
__pycache__			_virtualenv.py			anyio-4.6.2.post1.dist-info	idna-3.10.dist-info		sniffio-1.3.1.dist-info
_virtualenv.pth			anyio				idna				sniffio
❯ uv venv -p 3.11.5 --allow-existing
Using CPython 3.11.5
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ ls .venv/lib/python3.11/site-packages
__pycache__			_virtualenv.py			anyio-4.6.2.post1.dist-info	idna-3.10.dist-info		sniffio-1.3.1.dist-info
_virtualenv.pth			anyio				idna				sniffio
❯ .venv/bin/python --version
Python 3.11.5
```

Switching minor versions can change the package compatibility and is generally not safe.

---

_Comment by @janosh on 2025-01-14 20:49_

> and it's sort of a workaround

very useful! 👍  would be great to document this workaround and maybe add a test for it to ensure it doesn't break

---
