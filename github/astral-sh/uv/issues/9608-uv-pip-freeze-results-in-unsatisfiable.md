---
number: 9608
title: "`uv pip freeze` results in unsatisfiable requirements"
type: issue
state: closed
author: augustebaum
labels:
  - question
assignees: []
created_at: 2024-12-03T15:20:02Z
updated_at: 2024-12-06T00:33:39Z
url: https://github.com/astral-sh/uv/issues/9608
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip freeze` results in unsatisfiable requirements

---

_Issue opened by @augustebaum on 2024-12-03 15:20_

This seems like it might a known issue, but my search for similar issues has been unsuccessful so far.

Long story short:
```sh
$ uv pip freeze --verbose
DEBUG uv 0.5.5
DEBUG Searching for default Python interpreter in virtual environments or search path
DEBUG Found `cpython-3.12.5-linux-x86_64-gnu` at `/home/auguste/Desktop/skore/.venv/bin/python3` (active virtual environment)
Using Python 3.12.5 environment at: /home/auguste/Desktop/skore/.venv
...
jsonschema-specifications==2023.12.1
jsonschema-specifications==2024.10.1
...
```

I'm on NixOS with uv 0.5.5. Please let me know if there's any other information I can give.

---

_Comment by @zanieb on 2024-12-03 15:23_

It's possible for your environment to have multiple versions of a package installed, since each `pip install` operation is independent. I think it's correct for us to emit both in this case.



---

_Label `question` added by @zanieb on 2024-12-03 15:23_

---

_Comment by @zanieb on 2024-12-03 15:24_

Generally, the best way to avoid this is use a higher-level interface like `uv add`, `uv sync`, and `uv export` which will not allow multiple versions.

---

_Closed by @charliermarsh on 2024-12-06 00:33_

---
