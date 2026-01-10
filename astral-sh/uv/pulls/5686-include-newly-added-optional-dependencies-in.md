```yaml
number: 5686
title: Include newly-added optional dependencies in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/extra-add
created_at: 2024-08-01T14:47:05Z
updated_at: 2024-08-01T16:41:39Z
url: https://github.com/astral-sh/uv/pull/5686
synced_at: 2026-01-10T13:37:23Z
```

# Include newly-added optional dependencies in lockfile

---

_Pull request opened by @charliermarsh on 2024-08-01 14:47_

## Summary

When we add a new optional group in `uv add`, we never to update the `pyproject.toml` before locking. Otherwise, we use the stale `pyproject.toml` and omit the optional group.

Closes https://github.com/astral-sh/uv/issues/5687.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-01 14:47_

---

_Label `bug` added by @charliermarsh on 2024-08-01 14:47_

---

_Label `preview` added by @charliermarsh on 2024-08-01 14:47_

---

_@zanieb reviewed on 2024-08-01 14:50_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:935 on 2024-08-01 14:50_

A comment explaining this result would be helpful.

---

_@charliermarsh reviewed on 2024-08-01 14:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/edit.rs`:935 on 2024-08-01 14:53_

No clue

---

_@charliermarsh reviewed on 2024-08-01 14:54_

---

_Review comment by @charliermarsh on `crates/uv/tests/edit.rs`:935 on 2024-08-01 14:54_

Oh, I guess `add` includes all optionals but `sync` does not.

---

_@charliermarsh reviewed on 2024-08-01 14:55_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject_mut.rs`:49 on 2024-08-01 14:55_

I feel like there should be a more efficient way to do this? We don't even need the full-fidelity string here.

---

_Merged by @charliermarsh on 2024-08-01 16:41_

---

_Closed by @charliermarsh on 2024-08-01 16:41_

---

_Branch deleted on 2024-08-01 16:41_

---
