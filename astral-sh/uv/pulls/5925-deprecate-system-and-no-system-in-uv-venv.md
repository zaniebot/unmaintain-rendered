```yaml
number: 5925
title: "Deprecate `--system` and `--no-system` in `uv venv`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/venv-system
created_at: 2024-08-08T18:23:09Z
updated_at: 2024-08-08T18:32:01Z
url: https://github.com/astral-sh/uv/pull/5925
synced_at: 2026-01-10T13:31:54Z
```

# Deprecate `--system` and `--no-system` in `uv venv`

---

_Pull request opened by @zanieb on 2024-08-08 18:23_

e.g.

```
❯ cargo run -- venv --no-system
    Blocking waiting for file lock on build directory
   Compiling uv v0.2.34 (/Users/zb/workspace/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 19.85s
     Running `target/debug/uv venv --no-system`
warning: The `--no-system` flag has no effect, a system Python interpreter is always used in `uv venv`
Using Python 3.12.4 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ cargo run -- venv --system
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv venv --system`
warning: The `--system` flag has no effect, a system Python interpreter is always used in `uv venv`
Using Python 3.12.4 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

---

_Label `cli` added by @zanieb on 2024-08-08 18:23_

---

_@charliermarsh approved on 2024-08-08 18:23_

---

_Merged by @zanieb on 2024-08-08 18:32_

---

_Closed by @zanieb on 2024-08-08 18:32_

---

_Branch deleted on 2024-08-08 18:32_

---
