```yaml
number: 8890
title: Improve Python discovery source messages
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/preference-format
created_at: 2024-11-07T15:58:49Z
updated_at: 2024-11-07T20:08:52Z
url: https://github.com/astral-sh/uv/pull/8890
synced_at: 2026-01-12T16:08:32Z
```

# Improve Python discovery source messages

---

_@zanieb_

e.g.

```
❯ echo "anyio" |  cargo run -q -- pip compile - -v
DEBUG uv 0.4.30 (107ab3d71 2024-11-07)
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
```
```
❯ cargo run -q -- pip install anyio -v
DEBUG uv 0.4.30 (107ab3d71 2024-11-07)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
```

vs

```
❯ uv  pip install anyio -v
DEBUG uv 0.4.30 (61ed2a236 2024-11-04)
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
```

```
❯ echo "anyio" | uv pip compile - -v
DEBUG uv 0.4.30 (61ed2a236 2024-11-04)
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
```

---

_Label `tracing` added by @zanieb on 2024-11-07 15:58_

---

_@zanieb reviewed on 2024-11-07 17:44_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:137 on 2024-11-07 17:44_

In a later pull request, I'll consolidate methods to take this type instead of the separate preferences.

---

_Marked ready for review by @zanieb on 2024-11-07 17:45_

---

_Merged by @zanieb on 2024-11-07 20:08_

---

_Closed by @zanieb on 2024-11-07 20:08_

---

_Branch deleted on 2024-11-07 20:08_

---
