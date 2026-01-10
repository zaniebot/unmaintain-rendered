```yaml
number: 8364
title: Log netrc parsing error
type: pull_request
state: merged
author: j178
labels:
  - error messages
assignees: []
merged: true
base: main
head: netrc-warning
created_at: 2024-10-19T13:47:57Z
updated_at: 2024-10-23T05:34:47Z
url: https://github.com/astral-sh/uv/pull/8364
synced_at: 2026-01-10T12:54:08Z
```

# Log netrc parsing error

---

_Pull request opened by @j178 on 2024-10-19 13:47_

## Summary

Resolves #7685

## Test Plan

```console
$ echo "this is an invalid netrc" > .netrc
$ NETRC=.netrc cargo run -- pip install anyio --index-url https://pypi-proxy.fly.dev/basic-auth/simple --strict -v
DEBUG uv 0.4.24 (f4d5fba61 2024-10-19)
DEBUG Searching for default Python interpreter in system path or `py` launcher
DEBUG Found `cpython-3.11.2-windows-x86_64-none` at `D:\Projects\Rust\uv\.venv\Scripts\python.exe` (virtual environment)
DEBUG Using Python 3.11.2 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: anyio
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.2
DEBUG Solving with target Python version: >=3.11.2
DEBUG Adding direct dependency: anyio*
DEBUG No cache entry for: https://pypi-proxy.fly.dev/basic-auth/simple/anyio/
WARN Error reading netrc file: parsing error: bad toplevel token 'this' (line 1) in the file '.netrc'
DEBUG Searching for a compatible version of anyio (*)
DEBUG No compatible version found for: anyio
  × No solution found when resolving dependencies:
  ╰─▶ Because anyio was not found in the package registry and you require anyio, we can conclude that your
      requirements are unsatisfiable.

      hint: An index URL (https://pypi-proxy.fly.dev/basic-auth/simple) could not be queried due to a lack of valid
      authentication credentials (401 Unauthorized).
DEBUG Released lock at `D:\Projects\Rust\uv\.venv\.lock`
error: process didn't exit successfully: `target\debug\uv.exe pip install anyio --index-url https://pypi-proxy.fly.dev/basic-auth/simple --strict -v` (exit code: 1)

```

---

_Marked ready for review by @j178 on 2024-10-19 13:56_

---

_Converted to draft by @j178 on 2024-10-19 14:25_

---

_@j178 reviewed on 2024-10-19 14:30_

---

_Review comment by @j178 on `crates/uv-auth/src/middleware.rs`:402 on 2024-10-19 14:30_

Should we use `warn_user_once!` here instead?

---

_@j178 reviewed on 2024-10-19 14:31_

---

_Review comment by @j178 on `crates/uv-auth/src/middleware.rs`:386 on 2024-10-19 14:31_

We shouldn't parse netrc for each request.

---

_Marked ready for review by @j178 on 2024-10-19 14:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-20 16:12_

---

_@charliermarsh approved on 2024-10-20 16:27_

---

_Merged by @charliermarsh on 2024-10-20 16:27_

---

_Closed by @charliermarsh on 2024-10-20 16:27_

---

_Label `error messages` added by @charliermarsh on 2024-10-20 16:27_

---

_Branch deleted on 2024-10-23 05:34_

---
