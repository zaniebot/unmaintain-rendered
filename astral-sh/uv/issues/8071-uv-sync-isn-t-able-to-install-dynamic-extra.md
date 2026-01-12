```yaml
number: 8071
title: "`uv sync`  isn't able to install dynamic extra dependencies"
type: issue
state: closed
author: falckt
labels:
  - bug
assignees: []
created_at: 2024-10-10T05:44:58Z
updated_at: 2024-10-10T14:00:36Z
url: https://github.com/astral-sh/uv/issues/8071
synced_at: 2026-01-12T15:59:19Z
```

# `uv sync`  isn't able to install dynamic extra dependencies

---

_@falckt_

Setuptools supports dynamic optional requirements, see [here](https://github.com/kornia/kornia/blob/ec347a978d23103e16cd8b439f51e1a1b6d56f33/pyproject.toml#L74) for an example.

When running `uv sync --extra <optional>` or `uv sync --all-extras` these optional dependencies are not collected. Running `uv pip install .[<optional>]` does work though.

MWE
```command
> git clone https://github.com/kornia/kornia.git
> cd kornia
> uv sync --extra dev
Using CPython 3.11.9 interpreter at: /home/ubuntu/.local/share/mise/installs/python/3.11/bin/python
Creating virtual environment at: .venv
Resolved 25 packages in 492ms
Installed 25 packages in 224ms
[...]

> uv pip install ".[dev]"
Resolved 60 packages in 791ms
Uninstalled 1 package in 0.42ms
Installed 36 packages in 226ms
 + accelerate==1.0.0
 + certifi==2024.8.30
 + cfgv==3.4.0
 + charset-normalizer==3.4.0
 + coverage==7.6.2
[...]
```

I tested this using `uv 0.4.20` on ubuntu 22.04 running on x86_64.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-10 08:34_

---

_Comment by @charliermarsh on 2024-10-10 09:39_

Yeah `uv sync` and friends definitely don't support this. Honestly, I'm sort of undecided on whether we _should_.

---

_Label `bug` added by @charliermarsh on 2024-10-10 09:39_

---

_Label `needs-decision` added by @charliermarsh on 2024-10-10 09:39_

---

_Comment by @charliermarsh on 2024-10-10 09:40_

The basic issue is that we feed the set of members + their extras to the resolver, but if the extras aren't known upfront, they don't get included in the resolution. We'd need to reorder the resolver to resolve the members first.

---

_Comment by @charliermarsh on 2024-10-10 09:54_

We probably should support this, though it's annoying (and I personally don't recommend the use of dynamic metadata).

---

_Comment by @falckt on 2024-10-10 09:57_

No worries, from my side it has extremely low priority. Just stumbled over it and couldn't find an existing ticket. 

---

_Comment by @charliermarsh on 2024-10-10 10:13_

It took me a while to figure out why this was happening, but it does make sense to me now. Good bug!

---

_Label `needs-decision` removed by @charliermarsh on 2024-10-10 11:34_

---

_Closed by @charliermarsh on 2024-10-10 14:00_

---
