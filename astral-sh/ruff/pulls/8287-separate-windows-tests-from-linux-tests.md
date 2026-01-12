```yaml
number: 8287
title: Separate Windows tests from Linux tests
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/ci-split-win32
created_at: 2023-10-27T18:24:16Z
updated_at: 2023-10-27T20:11:37Z
url: https://github.com/astral-sh/ruff/pull/8287
synced_at: 2026-01-12T02:32:42Z
```

# Separate Windows tests from Linux tests

---

_Pull request opened by @zanieb on 2023-10-27 18:24_

Windows tests take much longer and downstream CI jobs that require the build from the Linux tests must wait to start.

Additionally, we already have if/else logic in the test suite for Windows tests which cannot run the same command.

This will require an update to the required checks in the repository settings.


---

_Label `internal` added by @zanieb on 2023-10-27 18:28_

---

_@charliermarsh approved on 2023-10-27 18:30_

I think that's reasonable given that downstream jobs depend on the Linux tests.

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:119 on 2023-10-27 18:36_

Git really struggling with the diff here. Just view the diffs of https://github.com/astral-sh/ruff/pull/8287/commits/ae3aa2032f247c1f20fd9b663a65212c30dac7d4 and https://github.com/astral-sh/ruff/pull/8287/commits/9735b7f98311d3521736dcdfdbaeedb1ec914168 separately. The later commit just swaps the fuzz/wasm test jobs.

---

_@zanieb reviewed on 2023-10-27 18:37_

---

_Merged by @zanieb on 2023-10-27 20:11_

---

_Closed by @zanieb on 2023-10-27 20:11_

---

_Branch deleted on 2023-10-27 20:11_

---
