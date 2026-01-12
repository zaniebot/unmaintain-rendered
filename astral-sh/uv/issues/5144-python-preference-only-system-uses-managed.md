```yaml
number: 5144
title: "`--python-preference only-system` uses managed interpreter if present in virtual environment"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-07-17T12:57:47Z
updated_at: 2025-07-17T22:20:26Z
url: https://github.com/astral-sh/uv/issues/5144
synced_at: 2026-01-12T15:58:54Z
```

# `--python-preference only-system` uses managed interpreter if present in virtual environment

---

_@konstin_

When a venv from a managed toolchain exists, uv ignores `--python-preference only-system`.

To reproduce, take a system without `python3.11` on path:

```toml
[project]
name = "foo"
version = "1"
requires-python = "==3.11.*"
dependencies = [
    "tqdm",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Create a managed venv, then sync again with `--python-preference only-system`. The venv is still managed python, which we can confirm by trying to recreate the venv:
```console
$ uv sync
Using Python 3.11.9 interpreter at: /home/konsti/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3
Creating virtualenv at: .venv
Resolved 3 packages in 5ms
   Built foo @ file:///home/konsti/projects/foo
Prepared 1 package in 120ms
Installed 2 packages in 1ms
 + foo==1 (from file:///home/konsti/projects/foo)
 + tqdm==4.66.4
$ uv sync --python-preference only-system
Resolved 3 packages in 4ms
Audited 2 packages in 0.10ms
$ rm -r .venv
$ uv sync --python-preference only-system
error: No interpreter found for Python >=3.11, <3.12 in system path
```

---

_Label `bug` added by @konstin on 2024-07-17 12:57_

---

_Label `preview` added by @konstin on 2024-07-17 12:57_

---

_Assigned to @zanieb by @zanieb on 2024-07-17 14:35_

---

_Comment by @zanieb on 2024-07-17 14:35_

Can you share verbose and trace logs?

---

_Comment by @zanieb on 2024-07-17 14:37_

Oh wait.. you're creating the virtual environment with the managed Python then requesting `only-system` in subsequent syncs? We don't perform discovery of an interpreter again since the environment already exists — this would only affect behavior if you also requested a new Python version.

I'm uncertain this should change.

---

_Renamed from "`--python-preference only-system` uses managed interpreter if existing" to "`--python-preference only-system` uses managed interpreter if present in virtual environment" by @zanieb on 2024-07-17 14:38_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Closed by @zanieb on 2025-07-17 22:20_

---
