```yaml
number: 2012
title: "Avoid SIM401 in `elif` blocks"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/sim401
created_at: 2023-01-20T02:48:52Z
updated_at: 2023-01-20T02:57:20Z
url: https://github.com/astral-sh/ruff/pull/2012
synced_at: 2026-01-12T15:55:07Z
```

# Avoid SIM401 in `elif` blocks

---

_@charliermarsh_

For now, we're just gonna avoid flagging this for `elif` blocks, following the same reasoning as for ternaries. We can handle all of these cases, but we'll knock out the TODOs as a pair, and this avoids broken code.

Closes #2007.

---

_Merged by @charliermarsh on 2023-01-20 02:57_

---

_Closed by @charliermarsh on 2023-01-20 02:57_

---

_Branch deleted on 2023-01-20 02:57_

---
