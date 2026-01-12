```yaml
number: 2563
title: Fix python module invocation
type: pull_request
state: merged
author: trottomv
labels: []
assignees: []
merged: true
base: main
head: fix-python-module-invocation
created_at: 2023-02-04T08:35:27Z
updated_at: 2023-02-04T21:47:26Z
url: https://github.com/astral-sh/ruff/pull/2563
synced_at: 2026-01-12T04:52:00Z
```

# Fix python module invocation

---

_Pull request opened by @trottomv on 2023-02-04 08:35_

This changes fix the #2520 Issue

---

_Renamed from "Fix python module invocation #2520" to "Fix python module invocation" by @trottomv on 2023-02-04 09:04_

---

_Merged by @charliermarsh on 2023-02-04 13:23_

---

_Closed by @charliermarsh on 2023-02-04 13:23_

---

_Branch deleted on 2023-02-04 16:45_

---

_Review comment by @andersk on `python/ruff/__main__.py`:7 on 2023-02-04 21:13_

I expect this is wrong on Windows. The right way to get the `--user` scripts path is `sysconfig.get_path("scripts", scheme=sysconfig.get_preferred_scheme("user"))`—but that requires Python ≥ 3.10, so we’ll need to figure out the right translation for older Python.

---

_Review comment by @andersk on `python/ruff/__main__.py`:8 on 2023-02-04 21:13_

We shouldn’t prefer a version from `pip install --user` *over* a currently active virtualenv.

---

_Review comment by @andersk on `python/ruff/__main__.py`:24 on 2023-02-04 21:13_

This ends up raising `FileNotFoundError(FileNotFoundError(ruff_path))`. Why catch and rewrap the exception at all?

---

_@andersk reviewed on 2023-02-04 21:22_

---

_@charliermarsh reviewed on 2023-02-04 21:27_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:7 on 2023-02-04 21:27_

Can you think of any other projects that've had to solve this problem?

---

_Review comment by @andersk on `python/ruff/__main__.py`:15 on 2023-02-04 21:47_

This doesn’t work at all on Windows, where the binary has a `.exe` extension (which had been automatically added by `os.spawnv`).

---

_@andersk reviewed on 2023-02-04 21:47_

Opened #2574 to fix all these issues.

---
