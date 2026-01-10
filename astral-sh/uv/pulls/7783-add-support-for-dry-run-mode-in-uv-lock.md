```yaml
number: 7783
title: "Add support for `--dry-run` mode in `uv lock`"
type: pull_request
state: merged
author: tfsingh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: tfsingh/dry
created_at: 2024-09-29T18:59:44Z
updated_at: 2024-10-24T03:22:25Z
url: https://github.com/astral-sh/uv/pull/7783
synced_at: 2026-01-10T12:53:55Z
```

# Add support for `--dry-run` mode in `uv lock`

---

_Pull request opened by @tfsingh on 2024-09-29 18:59_

This PR adds support for `uv lock --dry-run`, as described in issue #6408.

One thing to note: this functionality, as implemented, isn't limited to `-U` (if someone adds a dependency to the project's `pyproject.toml`, the plan will include these changes).

---

_@tfsingh reviewed on 2024-09-29 19:13_

---

_Review comment by @tfsingh on `crates/uv/src/commands/project/lock.rs`:756 on 2024-09-29 19:13_

If we don't do this check, when we use `--dry-run` with `-U` the caller (`do_lock`) will indicate there was an update even if there wasn't one.

I'm not the happiest with this change as it pollutes the `do_lock` and `validate` APIs. That being said, I'm not sure of a better approach (doesn't make sense to include dry_run as a variant/member of one of the enums/structs passed in imo, they all seem to be used quite widely) — open to suggestions!

---

_Marked ready for review by @tfsingh on 2024-09-30 16:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-30 16:45_

---

_@charliermarsh approved on 2024-10-24 03:13_

---

_Renamed from "Support uv lock --dry-run" to "Add support for `--dry-run` mode in `uv lock`" by @charliermarsh on 2024-10-24 03:13_

---

_Merged by @charliermarsh on 2024-10-24 03:21_

---

_Closed by @charliermarsh on 2024-10-24 03:21_

---

_Label `enhancement` added by @charliermarsh on 2024-10-24 03:22_

---

_Label `cli` added by @charliermarsh on 2024-10-24 03:22_

---
