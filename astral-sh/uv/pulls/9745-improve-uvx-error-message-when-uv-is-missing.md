```yaml
number: 9745
title: Improve uvx error message when uv is missing
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/uvx-missing
created_at: 2024-12-09T19:13:57Z
updated_at: 2025-01-21T23:06:58Z
url: https://github.com/astral-sh/uv/pull/9745
synced_at: 2026-01-10T11:44:30Z
```

# Improve uvx error message when uv is missing

---

_Pull request opened by @zanieb on 2024-12-09 19:13_

from https://github.com/astral-sh/uv/issues/9742

```
❯ cargo run -q --bin uvx
Provide a command to run with `uvx <command>`.

The following tools are installed:

- ansible-core v2.17.5
- black v24.10.0
- rooster-blue v0.0.0

See `uvx --help` for more information.
❯ rm target/debug/uv
❯ cargo run -q --bin uvx
error: Could not find the `uv` binary at /Users/zb/workspace/uv/target/debug/uv
```

---

_Label `error messages` added by @zanieb on 2024-12-09 19:13_

---

_@charliermarsh approved on 2024-12-09 19:14_

---

_Comment by @charliermarsh on 2024-12-09 19:14_

We could give some guidance on how to fix, but not blocking.

---

_Comment by @zanieb on 2024-12-09 19:19_

Yeah idk what the guidance is? Reinstall?

---

_Comment by @charliermarsh on 2024-12-09 19:20_

Yeah I think at least `rm` the `uvx`...

---

_Comment by @zanieb on 2024-12-09 19:22_

I'm interested in waiting to see how someone would get in this situation, but agree guidance would be nice.

---

_Merged by @zanieb on 2025-01-21 23:06_

---

_Closed by @zanieb on 2025-01-21 23:06_

---

_Branch deleted on 2025-01-21 23:06_

---
