```yaml
number: 1946
title: "Exempt `contextlib.ExitStack()` for SIM115 rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/sim115
created_at: 2023-01-18T03:38:59Z
updated_at: 2023-01-18T03:39:56Z
url: https://github.com/astral-sh/ruff/pull/1946
synced_at: 2026-01-12T15:55:07Z
```

# Exempt `contextlib.ExitStack()` for SIM115 rules

---

_@charliermarsh_

Since our binding tracking is somewhat limited, I opted to favor false negatives over false positives. So, e.g., this won't trigger SIM115:

```py
with contextlib.ExitStack():
    f = exit_stack.enter_context(open("filename"))
```

(Notice that `exit_stack` is unbound.)

The alternative strategy required us to incorrectly trigger SIM115 on this:

```py
with contextlib.ExitStack() as exit_stack:
    exit_stack_ = exit_stack
    f = exit_stack_.enter_context(open("filename"))
```

Closes #1945.


---

_Merged by @charliermarsh on 2023-01-18 03:39_

---

_Closed by @charliermarsh on 2023-01-18 03:39_

---

_Branch deleted on 2023-01-18 03:39_

---
