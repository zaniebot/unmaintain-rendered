```yaml
number: 846
title: Upgrade maturin to 0.14
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: upgrade-maturin
created_at: 2022-11-21T05:02:59Z
updated_at: 2022-11-21T15:01:04Z
url: https://github.com/astral-sh/ruff/pull/846
synced_at: 2026-01-12T15:55:05Z
```

# Upgrade maturin to 0.14

---

_@messense_

_No description provided._

---

_@messense reviewed on 2022-11-21 05:03_

---

_Review comment by @messense on `.github/workflows/flake8-to-ruff.yaml`:37 on 2022-11-21 05:03_

maturin-action by default read maturin version from `pyproject.toml` and install a semver compatible version, see https://github.com/PyO3/maturin-action/pull/46

---

_@messense reviewed on 2022-11-21 05:04_

---

_Review comment by @messense on `pyproject.toml`:33 on 2022-11-21 05:04_

`Cargo.lock` is now included in sdist by default.

---

_Comment by @charliermarsh on 2022-11-21 15:00_

Awesome, thank you!

---

_Merged by @charliermarsh on 2022-11-21 15:00_

---

_Closed by @charliermarsh on 2022-11-21 15:00_

---

_Branch deleted on 2022-11-21 15:01_

---
