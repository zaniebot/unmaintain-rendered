```yaml
number: 5471
title: "Add `--isolated` support to `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/isolate-run
created_at: 2024-07-26T00:51:52Z
updated_at: 2024-07-30T19:27:49Z
url: https://github.com/astral-sh/uv/pull/5471
synced_at: 2026-01-10T13:37:23Z
```

# Add `--isolated` support to `uv run`

---

_Pull request opened by @charliermarsh on 2024-07-26 00:51_

## Summary

The culmination of #4730. We now have `uv run --isolated` which always uses a fresh environment (but includes the workspace dependencies as needed). This enables you to test with strict isolation (e.g., `uv run --isolated -p foo` will ensure that `foo` is unable to import anything that isn't an actual dependency).

Closes #5430.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-26 00:51_

---

_Review requested from @konstin by @charliermarsh on 2024-07-26 00:51_

---

_Label `cli` added by @charliermarsh on 2024-07-26 00:51_

---

_Label `preview` added by @charliermarsh on 2024-07-26 00:51_

---

_@charliermarsh reviewed on 2024-07-26 00:52_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1909 on 2024-07-26 00:52_

Explaining the difference between `--isolate` and `--no-workspace` is fairly nuanced, though I believe that both are necessary, they serve very different purposes.


---

_@konstin approved on 2024-07-26 09:30_

---

_@zanieb approved on 2024-07-30 13:38_

---

_Renamed from "Add `--isolate` support to `uv run`" to "Add `--isolated` support to `uv run`" by @charliermarsh on 2024-07-30 19:25_

---

_Merged by @charliermarsh on 2024-07-30 19:27_

---

_Closed by @charliermarsh on 2024-07-30 19:27_

---

_Branch deleted on 2024-07-30 19:27_

---
