```yaml
number: 16080
title: Confirm that the directory name is a valid Python install key during managed check
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/is-managed-key
created_at: 2025-09-30T22:18:54Z
updated_at: 2025-10-03T19:36:10Z
url: https://github.com/astral-sh/uv/pull/16080
synced_at: 2026-01-12T16:12:05Z
```

# Confirm that the directory name is a valid Python install key during managed check

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/16077

```
❯ UV_PYTHON_INSTALL_DIR=/opt uv python list --no-managed-python
cpython-3.9.6-macos-aarch64-none    /usr/bin/python3
❯ alias uv=$(pwd)/target/debug/uv
❯ UV_PYTHON_INSTALL_DIR=/opt uv python list --no-managed-python
cpython-3.13.3-macos-aarch64-none     /opt/homebrew/bin/python3.13 -> ../Cellar/python@3.13/3.13.3/bin/python3.13
cpython-3.13.3-macos-aarch64-none     /opt/homebrew/bin/python3 -> ../Cellar/python@3.13/3.13.3/bin/python3
cpython-3.12.10-macos-aarch64-none    /opt/homebrew/bin/python3.12 -> ../Cellar/python@3.12/3.12.10/bin/python3.12
cpython-3.11.12-macos-aarch64-none    /opt/homebrew/bin/python3.11 -> ../Cellar/python@3.11/3.11.12/bin/python3.11
cpython-3.9.6-macos-aarch64-none      /usr/bin/python3
pypy-3.10.14-macos-aarch64-none       /opt/homebrew/bin/pypy3 -> ../Cellar/pypy3.10/7.3.17_1/bin/pypy3
```

Prevents false positives when the `UV_PYTHON_INSTALL_DIR` is a prefix of some other path with Python installations

---

_Label `bug` added by @zanieb on 2025-09-30 22:19_

---

_Marked ready for review by @zanieb on 2025-10-01 14:05_

---

_@charliermarsh approved on 2025-10-03 02:04_

---

_@charliermarsh reviewed on 2025-10-03 02:05_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:313 on 2025-10-03 02:05_

Nit: you could probably avoid `to_string_lossy`, I assume it's always invalid if it's not UTF-8?

---

_Merged by @zanieb on 2025-10-03 19:36_

---

_Closed by @zanieb on 2025-10-03 19:36_

---

_Branch deleted on 2025-10-03 19:36_

---
