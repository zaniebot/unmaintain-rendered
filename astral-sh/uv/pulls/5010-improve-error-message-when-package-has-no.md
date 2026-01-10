```yaml
number: 5010
title: Improve error message when package has no installation candidates
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/neither-wheels-nor-source-dist
created_at: 2024-07-12T14:46:17Z
updated_at: 2024-07-15T00:00:41Z
url: https://github.com/astral-sh/uv/pull/5010
synced_at: 2026-01-10T13:42:52Z
```

# Improve error message when package has no installation candidates

---

_Pull request opened by @konstin on 2024-07-12 14:46_

Currently, with
```toml
[project]
name = "transformers"
version = "4.39.0.dev0"
requires-python = ">=3.10"
dependencies = [
  "torch==1.10.0"
]
```
i get
```
$ uv sync --preview
Resolved 3 packages in 7ms
error: found distribution torch==1.10.0 @ registry+https://pypi.org/simple with neither wheels nor source distribution
```
This error message is wrong, there are wheels, they are just not compatible. I initially got this error message during `uv lock` (in a build), so i also added that this is about installation, not about locking.

We should reject this version immediately because with the current requires python, it can never be installed, but even then we need to change the error message because you can be on the correct python version, but an unsupported platform.


---

_Label `error messages` added by @konstin on 2024-07-12 14:46_

---

_@ibraheemdev approved on 2024-07-12 15:38_

---

_@charliermarsh reviewed on 2024-07-12 18:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:2245 on 2024-07-12 18:06_

"can't be installed because it doesn't have a source distribution or wheel for the current platform"

---

_Merged by @charliermarsh on 2024-07-15 00:00_

---

_Closed by @charliermarsh on 2024-07-15 00:00_

---

_Branch deleted on 2024-07-15 00:00_

---
