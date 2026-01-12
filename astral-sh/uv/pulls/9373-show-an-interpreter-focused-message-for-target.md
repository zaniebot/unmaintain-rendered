```yaml
number: 9373
title: "Show an interpreter-focused message for `--target` and `--prefix`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/target-message
created_at: 2024-11-23T00:32:08Z
updated_at: 2024-11-27T14:21:47Z
url: https://github.com/astral-sh/uv/pull/9373
synced_at: 2026-01-12T16:08:47Z
```

# Show an interpreter-focused message for `--target` and `--prefix`

---

_@charliermarsh_

## Summary

With `uv pip install --target` and `--prefix`, we (1) should allow managed Pythons, and (2) should show a different message that's focused on the interpreter we selected, rather than the environment.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-23 00:32_

---

_Label `cli` added by @charliermarsh on 2024-11-23 00:32_

---

_@charliermarsh reviewed on 2024-11-23 00:32_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:584 on 2024-11-23 00:32_

Pretty clunky but like... in `uv venv`, we show this at full color, with cyan paths. In `uv pip install`, we show it dimmed. (This is largely preserving existing behavior, open to changing one of the two.)

---

_Review request for @zanieb removed by @charliermarsh on 2024-11-26 21:25_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-26 21:25_

---

_@zanieb approved on 2024-11-26 22:32_

Nice <3

---

_Merged by @charliermarsh on 2024-11-27 14:21_

---

_Closed by @charliermarsh on 2024-11-27 14:21_

---

_Branch deleted on 2024-11-27 14:21_

---
