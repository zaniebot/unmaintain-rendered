```yaml
number: 6361
title: "Respect `.python-version` files in `uv run` outside projects"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/run-python-pin
created_at: 2024-08-21T17:41:17Z
updated_at: 2024-08-21T21:41:29Z
url: https://github.com/astral-sh/uv/pull/6361
synced_at: 2026-01-10T13:09:51Z
```

# Respect `.python-version` files in `uv run` outside projects

---

_Pull request opened by @zanieb on 2024-08-21 17:41_

Closes https://github.com/astral-sh/uv/issues/6285

Introduces a new problem if the user says `python` but it doesn't exist with that name:

```
❯ cargo run -q -- -v run python --version
DEBUG uv 0.3.0
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG No project found; searching for Python interpreter
DEBUG Reading requests from `/Users/zb/workspace/uv/.python-version`
DEBUG Searching for Python 3.11 in managed installations or system path
DEBUG Found `cpython-3.12.4-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Searching for managed installations at `/Users/zb/Library/Application Support/uv/python`
DEBUG Found `cpython-3.11.9-macos-aarch64-none` at `/opt/homebrew/bin/python3.11` (search path)
DEBUG Using Python 3.11.9 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
DEBUG Running `python --version`
error: Failed to spawn: `python`
  Caused by: No such file or directory (os error 2)
```

I'll fix this separately.

---

_Label `bug` added by @zanieb on 2024-08-21 17:41_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-21 18:15_

---

_@charliermarsh approved on 2024-08-21 21:31_

---

_Merged by @zanieb on 2024-08-21 21:41_

---

_Closed by @zanieb on 2024-08-21 21:41_

---

_Branch deleted on 2024-08-21 21:41_

---
