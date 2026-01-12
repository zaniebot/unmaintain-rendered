```yaml
number: 2574
title: Portably find ruff binary path from Python
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: find-ruff-bin
created_at: 2023-02-04T21:32:53Z
updated_at: 2023-02-04T22:20:09Z
url: https://github.com/astral-sh/ruff/pull/2574
synced_at: 2026-01-12T15:55:08Z
```

# Portably find ruff binary path from Python

---

_@andersk_

Prefer the version from a currently active virtualenv over a version from `pip install --user`.  Add the .exe extension on Windows, and find the path for `pip install --user` correctly on Windows.

---

_@andersk reviewed on 2023-02-04 21:41_

---

_Review comment by @andersk on `python/ruff/__main__.py`:13 on 2023-02-04 21:41_

Oh, but there’s a second problem: this doesn’t check the `.exe` extension on Windows.

---

_Comment by @charliermarsh on 2023-02-04 21:52_

Good to go?

---

_@charliermarsh reviewed on 2023-02-04 21:55_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:29 on 2023-02-04 21:55_

This looks great, I should borrow it for the VS Code extension.

---

_Comment by @andersk on 2023-02-04 22:18_

Yeah I just tested on Windows with `pip install --user`, so I think this is good.

---

_Comment by @charliermarsh on 2023-02-04 22:19_

Thank you very much.

---

_Merged by @charliermarsh on 2023-02-04 22:19_

---

_Closed by @charliermarsh on 2023-02-04 22:19_

---

_Branch deleted on 2023-02-04 22:20_

---
