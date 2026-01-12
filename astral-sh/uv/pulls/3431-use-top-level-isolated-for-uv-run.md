```yaml
number: 3431
title: "Use top-level `--isolated` for `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/isolated
created_at: 2024-05-07T17:08:43Z
updated_at: 2024-05-07T17:30:03Z
url: https://github.com/astral-sh/uv/pull/3431
synced_at: 2026-01-12T16:05:38Z
```

# Use top-level `--isolated` for `uv run`

---

_@charliermarsh_

## Summary

We already have a global `--isolated`, which means "ignore any on-disk configuration". I think we should reuse this for the "ignore the workspace" setting in `uv run`, rather than `--no-workspace`.

I've also merged the existing `--isolated` and `--no-workspace` behaviors in `uv run` into a single flag. We may not need separate flags for this, since the current intent seems to be "ignore the workspace environment"? Though we could always re-add later.

Closes https://github.com/astral-sh/uv/issues/3421.


---

_Review requested from @zanieb by @charliermarsh on 2024-05-07 17:08_

---

_Label `cli` added by @charliermarsh on 2024-05-07 17:10_

---

_Label `preview` added by @charliermarsh on 2024-05-07 17:10_

---

_@zanieb reviewed on 2024-05-07 17:28_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1829 on 2024-05-07 17:28_

This (behavior) was useful for testing, but oh well I can add it back if I need it.

---

_@zanieb approved on 2024-05-07 17:28_

---

_Merged by @charliermarsh on 2024-05-07 17:30_

---

_Closed by @charliermarsh on 2024-05-07 17:30_

---

_Branch deleted on 2024-05-07 17:30_

---
