```yaml
number: 4200
title: "Avoid crash for `XDG_CONFIG_HOME=/dev/null`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/toml
created_at: 2024-06-10T16:05:25Z
updated_at: 2024-06-10T17:36:30Z
url: https://github.com/astral-sh/uv/pull/4200
synced_at: 2026-01-12T16:06:05Z
```

# Avoid crash for `XDG_CONFIG_HOME=/dev/null`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/4199.

## Test Plan

`XDG_CONFIG_HOME=/dev/null cargo run venv`


---

_Label `bug` added by @charliermarsh on 2024-06-10 16:05_

---

_Review comment by @ibraheemdev on `crates/uv-workspace/src/workspace.rs`:29 on 2024-06-10 16:15_

Why this change? Just curious.

---

_@ibraheemdev approved on 2024-06-10 16:15_

---

_Review comment by @BurntSushi on `crates/uv-workspace/src/workspace.rs`:35 on 2024-06-10 16:15_

I might say, "... does not exist or is not a directory"

---

_@BurntSushi approved on 2024-06-10 16:15_

---

_@charliermarsh reviewed on 2024-06-10 16:16_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:29 on 2024-06-10 16:16_

Oops, mistake. I started by changing it to match against NotADirectory.

---

_Merged by @charliermarsh on 2024-06-10 17:36_

---

_Closed by @charliermarsh on 2024-06-10 17:36_

---

_Branch deleted on 2024-06-10 17:36_

---
