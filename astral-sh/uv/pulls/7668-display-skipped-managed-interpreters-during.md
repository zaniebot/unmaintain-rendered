```yaml
number: 7668
title: Display skipped managed interpreters during Python discovery
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/discovery-managed-skip
created_at: 2024-09-24T18:43:23Z
updated_at: 2024-09-24T19:41:44Z
url: https://github.com/astral-sh/uv/pull/7668
synced_at: 2026-01-10T12:53:52Z
```

# Display skipped managed interpreters during Python discovery

---

_Pull request opened by @zanieb on 2024-09-24 18:43_

e.g.

```
❯ cargo run -- python find 3.11rc2 -v
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv python find 3.11rc2 -v`
DEBUG uv 0.4.15
DEBUG Searching for Python 3.11rc2 in managed installations or system path
DEBUG Found `cpython-3.12.1-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (active virtual environment)
DEBUG Found `cpython-3.12.1-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Searching for managed installations at `/Users/zb/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.0rc2-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.12.1-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.11.7-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.13-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.9.18-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.18-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.12-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `pypy-3.9.19-macos-aarch64-none`
DEBUG Found `cpython-3.12.1-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python` (search path)
DEBUG Found `cpython-3.12.1-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (search path)
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Found `cpython-3.11.10-macos-aarch64-none` at `/opt/homebrew/bin/python3.11` (search path)
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
error: No interpreter found for Python 3.11rc2 in virtual environments, managed installations, or system path
```

---

_Label `tracing` added by @zanieb on 2024-09-24 18:43_

---

_Merged by @zanieb on 2024-09-24 19:41_

---

_Closed by @zanieb on 2024-09-24 19:41_

---

_Branch deleted on 2024-09-24 19:41_

---
