```yaml
number: 6588
title: Improve messages for empty solves and installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2024-08-24T16:48:20Z
updated_at: 2024-08-27T14:40:18Z
url: https://github.com/astral-sh/uv/pull/6588
synced_at: 2026-01-12T16:07:26Z
```

# Improve messages for empty solves and installs

---

_@charliermarsh_

## Summary

Tries to improve the following:

```
❯ cargo run sync
   Compiling uv-cli v0.0.1 (/Users/crmarsh/workspace/uv/crates/uv-cli)
   Compiling uv v0.3.3 (/Users/crmarsh/workspace/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.81s
     Running `/Users/crmarsh/workspace/uv/target/debug/uv sync`
Using Python 3.12.1
Creating virtualenv at: .venv
Resolved in 7ms
Audited environment in 0.05ms
```

In this case we don't actually have any dependencies -- should we just omit `Resolved in...` and perhaps even the audited line?


---

_Review requested from @zanieb by @charliermarsh on 2024-08-24 16:48_

---

_Label `cli` added by @charliermarsh on 2024-08-24 16:48_

---

_@zanieb approved on 2024-08-27 13:47_

---

_Merged by @charliermarsh on 2024-08-27 14:40_

---

_Closed by @charliermarsh on 2024-08-27 14:40_

---

_Branch deleted on 2024-08-27 14:40_

---
