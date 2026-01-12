```yaml
number: 1576
title: Warn user when D203 and D211 are enabled
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/warn
created_at: 2023-01-03T00:54:52Z
updated_at: 2023-01-03T16:37:26Z
url: https://github.com/astral-sh/ruff/pull/1576
synced_at: 2026-01-12T05:36:31Z
```

# Warn user when D203 and D211 are enabled

---

_Pull request opened by @charliermarsh on 2023-01-03 00:54_

Resolves #1575.

---

_Merged by @charliermarsh on 2023-01-03 00:54_

---

_Closed by @charliermarsh on 2023-01-03 00:54_

---

_Branch deleted on 2023-01-03 00:54_

---

_Comment by @ManonMarchand on 2023-01-03 15:49_

Hi, it looks like adding  `ignore = ["D203"]` or `ignore = ["D211"]`  to the `pyproject.toml` is not taken in account and thus does not remove the warning. 

I have no idea about what's happening.

---

_Comment by @ManonMarchand on 2023-01-03 15:57_

Also, setting 

```
[tool.ruff.pydocstyle]
convention = "numpy"  # Accepts: "google", "numpy", or "pep257"
```
Does not remove the warning. 
Should I open an issue?

---

_Comment by @charliermarsh on 2023-01-03 15:58_

Yeah, if you could open an issue with the full `pyproject.toml`, that'd be much appreciated and I can take a look later today.

---

_Comment by @ManonMarchand on 2023-01-03 16:37_

Thanks! I opened an issue. 

---
