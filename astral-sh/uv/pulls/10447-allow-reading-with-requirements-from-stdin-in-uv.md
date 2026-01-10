```yaml
number: 10447
title: "Allow reading `--with-requirements` from stdin in `uv add` and `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: zb/stdin
created_at: 2025-01-09T21:37:03Z
updated_at: 2025-01-09T22:39:40Z
url: https://github.com/astral-sh/uv/pull/10447
synced_at: 2026-01-10T11:44:49Z
```

# Allow reading `--with-requirements` from stdin in `uv add` and `uv run`

---

_Pull request opened by @zanieb on 2025-01-09 21:37_

For some reason this was banned when originally added (I did not see discussion about it). I think it's fine to allow. With `uv run`, there's a bit of nuance because we also allow the script to be read from stdin.

---

_Label `enhancement` added by @zanieb on 2025-01-09 21:37_

---

_Label `cli` added by @zanieb on 2025-01-09 21:37_

---

_@charliermarsh approved on 2025-01-09 22:34_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:95 on 2025-01-09 22:34_

Comment feels slightly misplaced but I guess it was here already.

---

_@charliermarsh reviewed on 2025-01-09 22:34_

---

_@zanieb reviewed on 2025-01-09 22:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:95 on 2025-01-09 22:39_

Yeah I think that's pretty separate.

---

_Merged by @zanieb on 2025-01-09 22:39_

---

_Closed by @zanieb on 2025-01-09 22:39_

---

_Branch deleted on 2025-01-09 22:39_

---
