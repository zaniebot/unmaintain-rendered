```yaml
number: 3115
title: "Allow `--python` and `--system` on `pip compile`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/pip-compile
created_at: 2024-04-18T04:45:03Z
updated_at: 2024-04-18T06:41:55Z
url: https://github.com/astral-sh/uv/pull/3115
synced_at: 2026-01-10T14:43:31Z
```

# Allow `--python` and `--system` on `pip compile`

---

_Pull request opened by @charliermarsh on 2024-04-18 04:45_

## Summary

I think these are useful to have for consistency, though the `--system` variant requires some new threading.

Closes: https://github.com/astral-sh/uv/issues/2242.


---

_Marked ready for review by @charliermarsh on 2024-04-18 04:45_

---

_Label `cli` added by @charliermarsh on 2024-04-18 04:45_

---

_@zanieb approved on 2024-04-18 04:49_

---

_@charliermarsh reviewed on 2024-04-18 04:54_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:430 on 2024-04-18 04:54_

Kind of wish this was `-p` for consistency with other commands, but `--python-version` is already `-p`.

---

_Merged by @charliermarsh on 2024-04-18 04:55_

---

_Closed by @charliermarsh on 2024-04-18 04:55_

---

_Branch deleted on 2024-04-18 04:55_

---

_Comment by @T-256 on 2024-04-18 06:41_

Why not make these flags global instead of setting them separately for all subcommands of `uv pip`?

---
