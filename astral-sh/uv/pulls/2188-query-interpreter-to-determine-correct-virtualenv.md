```yaml
number: 2188
title: "Query interpreter to determine correct `virtualenv` paths"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
  - virtualenv
assignees: []
merged: true
base: main
head: charlie/venv-paths
created_at: 2024-03-05T00:19:08Z
updated_at: 2024-03-05T21:13:25Z
url: https://github.com/astral-sh/uv/pull/2188
synced_at: 2026-01-10T14:54:43Z
```

# Query interpreter to determine correct `virtualenv` paths

---

_Pull request opened by @charliermarsh on 2024-03-05 00:19_

## Summary

This PR migrates our virtualenv creation from a setup that assumes prior knowledge of the correct paths, to a technique borrowed from `virtualenv` whereby we use `sysconfig` and `distutils` to determine the paths. The general trick is to grab the expected paths with `sysconfig`, then make them all relative, then make them absolute for a given directory.

Closes #2095.
Closes #2153.


---

_Marked ready for review by @charliermarsh on 2024-03-05 00:28_

---

_Label `compatibility` added by @charliermarsh on 2024-03-05 00:28_

---

_Label `virtualenv` added by @charliermarsh on 2024-03-05 00:28_

---

_Review requested from @konstin by @charliermarsh on 2024-03-05 00:44_

---

_@charliermarsh reviewed on 2024-03-05 01:01_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:407 on 2024-03-05 01:01_

E.g., this is no longer necessary.

---

_@konstin approved on 2024-03-05 09:46_

---

_Merged by @charliermarsh on 2024-03-05 21:13_

---

_Closed by @charliermarsh on 2024-03-05 21:13_

---

_Branch deleted on 2024-03-05 21:13_

---
