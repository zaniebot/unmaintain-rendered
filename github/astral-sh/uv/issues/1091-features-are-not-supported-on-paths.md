---
number: 1091
title: Features are not supported on paths
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2024-01-25T09:33:52Z
updated_at: 2024-01-26T12:07:53Z
url: https://github.com/astral-sh/uv/issues/1091
synced_at: 2026-01-07T13:12:16-06:00
---

# Features are not supported on paths

---

_Issue opened by @MichaReiser on 2024-01-25 09:33_

I followed [black's contributor instructions](https://black.readthedocs.io/en/latest/contributing/the_basics.html#technicalities) but used puffin instead of pip. 

```bash
black on  main [$] via 🐍 v3.11.6 
❯ puffin venv
Using Python 3.11.6 at /usr/bin/python3.11
Creating virtual environment at: .venv

black on  main [$] via 🐍 v3.11.6 
❯ source ./venv/bin/activate.fish

black on  main [$] via 🐍 v3.11.6 (venv) 
❯ puffin pip install -r test_requirements.txt
Audited 6 packages in 4ms

black on  main [$] via 🐍 v3.11.6 (venv) 
❯ puffin pip install -e .[d]
error: Failed to parse `.[d]`
  Caused by: failed to canonicalize path `./.[d]`
  Caused by: No such file or directory (os error 2)
```

As a user it's unclear if puffin doesn't support `.[d]` (I don't know what this is called) or if I mistyped the command. 

Puffin should tell users that it doesn't support this feature. Ideally, it would propose an alternative that it supports (replacement command)

---

_Label `enhancement` added by @MichaReiser on 2024-01-25 09:34_

---

_Renamed from "Using `puffin` for developing on Black" to "Features are not supported on paths" by @konstin on 2024-01-25 09:48_

---

_Label `enhancement` removed by @zanieb on 2024-01-25 14:51_

---

_Label `bug` added by @zanieb on 2024-01-25 14:51_

---

_Added to milestone `Initial release` by @zanieb on 2024-01-25 14:51_

---

_Comment by @charliermarsh on 2024-01-25 15:01_

I will fix this today.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-25 15:01_

---

_Referenced in [astral-sh/uv#1113](../../astral-sh/uv/pulls/1113.md) on 2024-01-26 00:17_

---

_Closed by @charliermarsh on 2024-01-26 12:07_

---
