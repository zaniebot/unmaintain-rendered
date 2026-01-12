```yaml
number: 12145
title: "`uv sync --script` only syncs when there are packages to add to env"
type: issue
state: closed
author: manzt
labels:
  - bug
assignees: []
created_at: 2025-03-13T00:57:15Z
updated_at: 2025-03-19T01:40:10Z
url: https://github.com/astral-sh/uv/issues/12145
synced_at: 2026-01-12T16:00:56Z
```

# `uv sync --script` only syncs when there are packages to add to env

---

_@manzt_

### Summary

I'm not entirely sure I understand the behavior, but it seems like there's a subtle bug where `uv sync --script` only syncs the environment when there are new dependencies to add. Specifically, removing dependencies and then syncing appears to be a no-op.

```sh
uv init --script blah.py
uv add --script blah.py polars
uv sync --script blah.py
# Creating script environment at: /Users/manzt/.cache/uv/environments-v2/blah-7cdfc916d13f4258
# Resolved 1 package in 12ms
# Installed 1 package in 6ms
#  + polars==1.24.0
uv remove --script blah.py polars
uv sync --script blah.py
# Using script environment at: /Users/manzt/.cache/uv/environments-v2/blah-7cdfc916d13f4258
#
uv add --script blah.py attrs
uv sync --script blah.py
# Using script environment at: /Users/manzt/.cache/uv/environments-v2/blah-7cdfc916d13f4258
# Resolved 1 package in 51ms
# Uninstalled 1 package in 15ms
# Installed 1 package in 3ms
#  + attrs==25.2.0
#  - polars==1.24.0
```

In the script above, I would expect `- polars==1.24.0` to appear after `uv remove --script blah.py polars`. I can confirm that polars is still in the environment until after the last `uv sync`.

### Platform

Darwin 23.6.0 arm64

### Version

uv 0.6.6 (c1a0bb85e 2025-03-12)

### Python version

Python 3.13

---

_Label `bug` added by @manzt on 2025-03-13 00:57_

---

_Comment by @charliermarsh on 2025-03-13 13:00_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-13 13:01_

---

_Renamed from "`uv sync --script` only sync when a new package is added" to "`uv sync --script` only syncs when there are packages to add to env" by @manzt on 2025-03-13 13:43_

---

_Comment by @charliermarsh on 2025-03-13 22:03_

Will take a look tonight.

---

_Comment by @manzt on 2025-03-13 23:18_

No rush at all. Thanks!

---

_Closed by @charliermarsh on 2025-03-14 00:41_

---

_Comment by @manzt on 2025-03-14 01:26_

whoa awesome

---

_Comment by @manzt on 2025-03-18 23:23_

Just want to say`uv sync --script` absolutely rocks.

For Jupyter notebooks, it's common to not know exactly what deps you might need for some analysis. Previously I was creating a fresh venv in the [juv-vscode](https://marketplace.visualstudio.com/items?itemName=manzt.juv) extension any time a package was added or removed. This meant clearing the existing venv, including module byte code, making users incur the same overhead of importing a library every time they restarted the Jupyter kernel after syncing the PEP 723 meta.

With `uv sync --script` its _such_ a better user experience. See in this example, importing `pandas` for the first time takes ~10s ("fresh" venv), but the second time (after adding anywidget, "synced" venv) and restarting the kernel its instant!

![Image](https://github.com/user-attachments/assets/dd2a39be-4461-4977-8012-24ca40979ea2)



---

_Comment by @charliermarsh on 2025-03-19 01:40_

Amazing, thank you for sharing!

---
