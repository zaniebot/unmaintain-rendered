```yaml
number: 5465
title: "Add `--no-workspace` and `--no-project` in lieu of `--isolated`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/no-project
created_at: 2024-07-26T00:10:08Z
updated_at: 2024-07-30T17:40:38Z
url: https://github.com/astral-sh/uv/pull/5465
synced_at: 2026-01-12T16:06:50Z
```

# Add `--no-workspace` and `--no-project` in lieu of `--isolated`

---

_@charliermarsh_

## Summary

Right now, `--isolated` is read from `uv run` and `uv init` to avoid discovering the current workspace (or project). This PR moves that behavior to a dedicated `--no-workspace` flag for `uv init`, and `--no-project` for `uv run`. They could use the same flag, but `--no-project` feels confusing for `uv init`, and `--no-workspace` seems confusing for `uv run` (especially so once you read the documentation, where we refer to the thing you're omitting as the project).

Closes https://github.com/astral-sh/uv/issues/5429.


---

_Marked ready for review by @charliermarsh on 2024-07-26 00:10_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-26 00:11_

---

_Review requested from @konstin by @charliermarsh on 2024-07-26 00:11_

---

_Label `cli` added by @charliermarsh on 2024-07-26 00:11_

---

_Label `preview` added by @charliermarsh on 2024-07-26 00:11_

---

_@konstin approved on 2024-07-26 09:31_

---

_Renamed from "Add `--no-workspace` in lieu of `--isolated`" to "Add `--no-workspace` and `--no-project` in lieu of `--isolated`" by @charliermarsh on 2024-07-29 17:41_

---

_Renamed from "Add `--no-workspace` and `--no-project` in lieu of `--isolated`" to "Add `--no-workspace` in lieu of `--isolated`" by @charliermarsh on 2024-07-29 17:42_

---

_Renamed from "Add `--no-workspace` in lieu of `--isolated`" to "Add `--no-workspace` and `--no-project` in lieu of `--isolated`" by @charliermarsh on 2024-07-29 17:49_

---

_@zanieb approved on 2024-07-30 13:34_

---

_Merged by @charliermarsh on 2024-07-30 17:40_

---

_Closed by @charliermarsh on 2024-07-30 17:40_

---

_Branch deleted on 2024-07-30 17:40_

---
