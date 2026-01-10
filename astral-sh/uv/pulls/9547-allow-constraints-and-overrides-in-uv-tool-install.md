```yaml
number: 9547
title: "Allow `--constraints` and `--overrides` in `uv tool install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/tool-constraint
created_at: 2024-12-01T03:39:15Z
updated_at: 2024-12-03T01:14:42Z
url: https://github.com/astral-sh/uv/pull/9547
synced_at: 2026-01-10T12:00:00Z
```

# Allow `--constraints` and `--overrides` in `uv tool install`

---

_Pull request opened by @charliermarsh on 2024-12-01 03:39_

## Summary

Closes https://github.com/astral-sh/uv/issues/9517.


---

_Label `cli` added by @charliermarsh on 2024-12-01 03:39_

---

_Comment by @charliermarsh on 2024-12-01 03:39_

I think `tool run` needs the same arguments.

---

_Review requested from @zanieb by @charliermarsh on 2024-12-01 03:41_

---

_@charliermarsh reviewed on 2024-12-02 03:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:368 on 2024-12-02 03:04_

Fixed in https://github.com/astral-sh/uv/pull/9570.

---

_@zanieb reviewed on 2024-12-02 23:22_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3800 on 2024-12-02 23:22_

Don't we want to be able to use `--override` for non-file overrides? e.g., `--override foo==0.1.0`

(same for `--constraint`)

---

_@zanieb approved on 2024-12-02 23:23_

---

_@charliermarsh reviewed on 2024-12-02 23:24_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3800 on 2024-12-02 23:24_

I don’t think we allow that elsewhere, but we could? How would we differentiate between file and non-file on the CLI?

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3800 on 2024-12-02 23:26_

I think `--override` vs `--overrides` would be the distinguishing factor.

I'm honestly not sure what the status quo is outside `uv pip`. I think there's a tracking issue about this somewhere, but not sure where...

---

_@zanieb reviewed on 2024-12-02 23:26_

---

_@zanieb reviewed on 2024-12-02 23:27_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3800 on 2024-12-02 23:27_

I guess in the long-term `--override` / `--overrides` vs `--override-file` / `--overrides-file` might make more sense? It's sort of awkward since we have the precedents from the pip interface.

---

_@zanieb reviewed on 2024-12-03 01:08_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3800 on 2024-12-03 01:08_

To be dealt with later :)

---

_Merged by @charliermarsh on 2024-12-03 01:14_

---

_Closed by @charliermarsh on 2024-12-03 01:14_

---

_Branch deleted on 2024-12-03 01:14_

---
