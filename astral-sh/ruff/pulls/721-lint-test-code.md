```yaml
number: 721
title: Lint test code
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: lint-tests
created_at: 2022-11-13T16:54:09Z
updated_at: 2022-11-13T19:22:31Z
url: https://github.com/astral-sh/ruff/pull/721
synced_at: 2026-01-12T05:48:45Z
```

# Lint test code

---

_Pull request opened by @harupy on 2022-11-13 16:54_

I found test code is not lint-checked.

---

_Merged by @charliermarsh on 2022-11-13 16:55_

---

_Closed by @charliermarsh on 2022-11-13 16:55_

---

_Comment by @charliermarsh on 2022-11-13 16:55_

Thanks!

---

_Branch deleted on 2022-11-13 16:56_

---

_Review comment by @andersk on `.github/workflows/ci.yaml`:80 on 2022-11-13 19:22_

`--all` should have been kept (or replaced with its modern synonym `--workspace`). Now we aren’t linting the `ruff_dev` and `flake8-to-ruff` crates.

- #725

---

_@andersk reviewed on 2022-11-13 19:22_

---
