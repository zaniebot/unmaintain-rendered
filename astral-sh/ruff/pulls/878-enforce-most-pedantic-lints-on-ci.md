```yaml
number: 878
title: Enforce most pedantic lints on CI
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/clippy
created_at: 2022-11-22T22:04:11Z
updated_at: 2022-11-22T23:55:58Z
url: https://github.com/astral-sh/ruff/pull/878
synced_at: 2026-01-12T05:48:46Z
```

# Enforce most pedantic lints on CI

---

_Pull request opened by @charliermarsh on 2022-11-22 22:04_

_No description provided._

---

_Comment by @charliermarsh on 2022-11-22 22:04_

@andersk - Thoughts?

---

_Comment by @andersk on 2022-11-22 22:24_

I think we want an allowlist rather than a denylist. These are 97 lints in `clippy::pedantic`, most of which we already satisfied and want to continue satisfying.

Also, the crate-level settings here won’t affect `flake8_to_ruff`, `ruff_dev`, or `src/main.rs`.

---

_Renamed from "Add pedantic lints to deny block" to "Enforce most pedantic lints on CI" by @charliermarsh on 2022-11-22 23:41_

---

_Merged by @charliermarsh on 2022-11-22 23:55_

---

_Closed by @charliermarsh on 2022-11-22 23:55_

---

_Branch deleted on 2022-11-22 23:55_

---
