```yaml
number: 2513
title: "Mark `--add-noqa` as incompatible with `--fix`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fix-add
created_at: 2023-02-03T04:39:37Z
updated_at: 2023-02-03T04:43:07Z
url: https://github.com/astral-sh/ruff/pull/2513
synced_at: 2026-01-12T04:52:00Z
```

# Mark `--add-noqa` as incompatible with `--fix`

---

_Pull request opened by @charliermarsh on 2023-02-03 04:39_

Closes #2187.

---

_Renamed from "Mark --add-noqa as incompatible with --fix" to "Mark `--add-noqa` as incompatible with `--fix`" by @charliermarsh on 2023-02-03 04:39_

---

_@charliermarsh reviewed on 2023-02-03 04:41_

---

_Review comment by @charliermarsh on `src/noqa.rs`:199 on 2023-02-03 04:41_

I don't wanna talk about it.

---

_Review comment by @charliermarsh on `ruff_cli/src/main.rs`:163 on 2023-02-03 04:41_

This is a rare path, since you can't set `--fix` on the CLI with `--add-noqa`. But if the user's `pyproject.toml` has `fix = true`, we warn.

---

_@charliermarsh reviewed on 2023-02-03 04:42_

---

_Merged by @charliermarsh on 2023-02-03 04:43_

---

_Closed by @charliermarsh on 2023-02-03 04:43_

---

_Branch deleted on 2023-02-03 04:43_

---
