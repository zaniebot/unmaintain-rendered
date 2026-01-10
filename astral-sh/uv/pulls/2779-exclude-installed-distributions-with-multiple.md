```yaml
number: 2779
title: Exclude installed distributions with multiple versions from consideration in the resolver
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-multiple-installed
created_at: 2024-04-02T16:23:38Z
updated_at: 2024-04-02T17:10:52Z
url: https://github.com/astral-sh/uv/pull/2779
synced_at: 2026-01-10T14:49:08Z
```

# Exclude installed distributions with multiple versions from consideration in the resolver

---

_Pull request opened by @zanieb on 2024-04-02 16:23_

Addresses panic introduced in #2596 and reported in https://github.com/astral-sh/uv/issues/2763#issuecomment-2030674936

When there are multiple versions of a package available, we remove the existing packages before installing the resolved version to "fix" the environment. We must remove all of the package versions and reinstall because removing _any_ of the package versions could break the others. Since reinstalls require a pull from the remote, this broke a contract between the resolver and the planner which must always agree on which packages should come from the remote. This further demonstrates that we should be constructing the install plan with more concrete knowledge from the resolver (i.e. `ResolvedDist` instead of `Requirement`) to avoid having to manually ensure logic matches.

## Test plan

Fails on `main` with panic succeeds on branch

```
uv venv --seed
source .venv/bin/activate
pip install anyio==3.7.0 --ignore-installed
pip install anyio==4.0.0 --ignore-installed
cargo run -- pip install anyio black -v
```

---

_Label `bug` added by @zanieb on 2024-04-02 16:23_

---

_Marked ready for review by @zanieb on 2024-04-02 16:30_

---

_Comment by @zanieb on 2024-04-02 16:30_

Investigating a unit test

---

_Review requested from @charliermarsh by @zanieb on 2024-04-02 16:31_

---

_@charliermarsh approved on 2024-04-02 16:45_

---

_Comment by @charliermarsh on 2024-04-02 16:46_

Feel free to add a test + merge (or I can re-review if you'd like).

---

_Merged by @zanieb on 2024-04-02 17:10_

---

_Closed by @zanieb on 2024-04-02 17:10_

---

_Branch deleted on 2024-04-02 17:10_

---
