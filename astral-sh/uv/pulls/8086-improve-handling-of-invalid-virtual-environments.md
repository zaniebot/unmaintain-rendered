```yaml
number: 8086
title: Improve handling of invalid virtual environments during interpreter discovery
type: pull_request
state: merged
author: potoo0
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-py-find
created_at: 2024-10-10T12:17:20Z
updated_at: 2024-12-11T02:50:06Z
url: https://github.com/astral-sh/uv/pull/8086
synced_at: 2026-01-10T11:59:59Z
```

# Improve handling of invalid virtual environments during interpreter discovery

---

_Pull request opened by @potoo0 on 2024-10-10 12:17_

## Summary

Fix #8075.

Invalid discovered environments in the working directory should be filtered out.

## Test Plan

- Test python_find



---

_Comment by @potoo0 on 2024-10-12 03:54_

This commit will also affect pip subcommand.

```
$ RUST_LOG=debug /tmp/uv pip install idna
DEBUG uv 0.4.20
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python`: system interpreter not explicitly requested
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python3`: system interpreter not explicitly requested
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/usr/bin/python3.12` (search path)
DEBUG Ignoring Python interpreter at `/usr/bin/python3.12`: system interpreter not explicitly requested
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment

$ RUST_LOG=debug uv pip install idna
DEBUG uv 0.4.20
DEBUG Searching for default Python interpreter in system path
error: Broken virtualenv `/home/code/.venv`: `pyvenv.cfg` is missing
```

```
$ RUST_LOG=debug uv pip list
DEBUG uv 0.4.20
DEBUG Searching for default Python interpreter in system path
error: Broken virtualenv `/home/code/.venv`: `pyvenv.cfg` is missing

$ RUST_LOG=debug /tmp/uv pip list
DEBUG uv 0.4.20
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/usr/bin/python` (search path)
Using Python 3.12.6 environment at /usr
Package            Version
------------------ ---------
autocommand        2.2.2
fail2ban           1.1.0
```

---

_Review requested from @zanieb by @charliermarsh on 2024-10-12 21:12_

---

_Assigned to @zanieb by @charliermarsh on 2024-10-12 21:12_

---

_Label `bug` added by @charliermarsh on 2024-10-12 21:12_

---

_Comment by @zanieb on 2024-10-14 18:25_

Ah we can't just filter there — that will change behavior too much.

Perhaps at https://github.com/astral-sh/uv/blob/715f28fd398aae1a6347deb312e3d0a27161696b/crates/uv-python/src/discovery.rs#L677 instead? I don't think this change is valid for other operations though.

---

_Comment by @zanieb on 2024-10-14 18:25_

Can you add a `python_find` test case covering this?

---

_Comment by @potoo0 on 2024-10-15 07:28_

@zanieb 

Maybe I'm using it incorrectly, I previously created virtual nesting under the .venv folder.
```bash
uv venv .venv/jupyter

tree -L 1 .venv
# .venv
# ├── huggingface
# ├── jupyter
# └── nogil
```


---

_@konstin approved on 2024-12-10 17:12_

---

_Renamed from "fix: drop invalid discovered environment(#8075)" to "Improve handling of invalid virtual environments during interpreter discovery" by @zanieb on 2024-12-10 18:47_

---

_Merged by @zanieb on 2024-12-10 18:56_

---

_Closed by @zanieb on 2024-12-10 18:56_

---

_Branch deleted on 2024-12-11 02:50_

---
