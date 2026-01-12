```yaml
number: 6241
title: "Rename `uv sync --no-clean` to `uv sync --inexact`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - breaking
  - preview
assignees: []
merged: true
base: release/3
head: zb/no-clean-no-more
created_at: 2024-08-20T02:40:13Z
updated_at: 2024-08-20T16:01:09Z
url: https://github.com/astral-sh/uv/pull/6241
synced_at: 2026-01-12T16:07:17Z
```

# Rename `uv sync --no-clean` to `uv sync --inexact`

---

_@zanieb_

_No description provided._

---

_Label `cli` added by @zanieb on 2024-08-20 02:40_

---

_Label `preview` added by @zanieb on 2024-08-20 02:40_

---

_Label `breaking` added by @zanieb on 2024-08-20 02:40_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-20 02:40_

---

_Comment by @zanieb on 2024-08-20 02:41_

I'm not convinced that we shouldn't perform an exact sync by default, and while I like the `--exact` / `--no-exact` names — I don't feel that we should adjust the default behavior because it's easier to write the names of the flag. 

I worry `--no-exact` is not obvious enough on its own.

---

_Comment by @zanieb on 2024-08-20 02:42_

(I don't love this name either, but it cannot remain `--no-clean` due to the conflict with cache clean semantics)

---

_Comment by @zanieb on 2024-08-20 05:10_

- `--no-prune`
- `--retain`
- `--preserve-extras` (conflicts with the optional deps concept)
- `--preserve`
- `--keep-unused`
- `--skip-prune`

---

_Comment by @notatallshaw-gts on 2024-08-20 14:12_

`--no-remove`
`--no-delete` (taken from rsync "delete" terminology)

---

_Comment by @zanieb on 2024-08-20 14:14_

But we _will_ remove packages if they conflict with the requirements of the project.

---

_Comment by @charliermarsh on 2024-08-20 14:17_

`--ignore-unused`? `--skip-unused`?

---

_Comment by @zanieb on 2024-08-20 14:39_

I worry a little about "unused" since they could be used just not declared.

---

_Renamed from "Rename `uv sync --no-clean` to `uv sync --keep-untracked`" to "Rename `uv sync --no-clean` to `uv sync --inexact`" by @zanieb on 2024-08-20 15:37_

---

_Comment by @zanieb on 2024-08-20 15:49_

Opting for `--inexact` with a `--no-exact` alias.

---

_@charliermarsh approved on 2024-08-20 15:52_

---

_Merged by @zanieb on 2024-08-20 16:01_

---

_Closed by @zanieb on 2024-08-20 16:01_

---

_Branch deleted on 2024-08-20 16:01_

---
