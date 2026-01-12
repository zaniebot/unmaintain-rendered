```yaml
number: 10717
title: "Omit `stdout` and `stderr` sections when empty in errors"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - error messages
  - tracing
assignees: []
created_at: 2025-01-17T17:45:14Z
updated_at: 2025-01-21T21:45:30Z
url: https://github.com/astral-sh/uv/issues/10717
synced_at: 2026-01-12T16:00:19Z
```

# Omit `stdout` and `stderr` sections when empty in errors

---

_@zanieb_

e.g., https://github.com/astral-sh/uv/blob/4d3809cc6b28d71f26a782929b994abfebd7973c/crates/uv-python/src/interpreter.rs#L565-L578

can create some sort of confusing output

    DEBUG uv [VERSION] ([COMMIT] DATE)
    DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
    DEBUG Failed to inspect Python interpreter from active virtual environment at `.venv/bin/python3`
    DEBUG Skipping bad interpreter at /Users/zb/.local/share/uv/tests/.tmpdujfqj/temp/.venv/bin/python3 from active virtual environment: Querying Python at `/Users/zb/.local/share/uv/tests/.tmpdujfqj/temp/.venv/bin/python3` failed with exit status exit status: 1
    --- stdout:

    --- stderr:

    ---
    DEBUG Failed to inspect Python interpreter from virtual environment at `.venv/bin/python3`
    DEBUG Skipping bad interpreter at /Users/zb/.local/share/uv/tests/.tmpdujfqj/temp/.venv/bin/python3 from virtual environment: Querying Python at `/Users/zb/.local/share/uv/tests/.tmpdujfqj/temp/.venv/bin/python3` failed with exit status exit status: 1
    --- stdout:

    --- stderr:

    ---
    DEBUG Searching for managed installations at `.`
    DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/Users/zb/.local/share/uv/tests/.tmpdujfqj/python/3.12/python3` (search path)

---

_Label `error messages` added by @zanieb on 2025-01-17 17:45_

---

_Label `tracing` added by @zanieb on 2025-01-17 17:45_

---

_Comment by @charliermarsh on 2025-01-17 18:34_

We do a similar thing for the build backend errors, if helpful...

---

_Comment by @zanieb on 2025-01-17 18:36_

ref https://github.com/astral-sh/uv/blob/9e0b35ad8280924fc9e347f668d6153d925673de/crates/uv-build-frontend/src/error.rs#L254-L291

---

_Label `help wanted` added by @zanieb on 2025-01-17 18:36_

---

_Closed by @charliermarsh on 2025-01-21 21:45_

---

_Closed by @charliermarsh on 2025-01-21 21:45_

---
