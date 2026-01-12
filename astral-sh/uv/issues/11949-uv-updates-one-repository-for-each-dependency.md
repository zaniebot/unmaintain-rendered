```yaml
number: 11949
title: UV updates one repository for each dependency
type: issue
state: closed
author: Wintreist
labels:
  - bug
assignees: []
created_at: 2025-03-04T14:49:20Z
updated_at: 2025-03-04T15:07:07Z
url: https://github.com/astral-sh/uv/issues/11949
synced_at: 2026-01-12T16:00:50Z
```

# UV updates one repository for each dependency

---

_@Wintreist_

### Summary

pyproject.toml:
```
...
[tool.uv.sources]
lib_1 = { git = "ssh://git@repo_1.git", branch = "branch_1" }
lib_2 = { git = "ssh://git@repo_3.git", subdirectory = "lib_2", branch = "branch_2" }
lib_3 = { git = "ssh://git@repo_2.git" }
lib_4 = { git = "ssh://git@repo_3.git", subdirectory = "lib_4", branch = "branch_2" }
lib_5 = { git = "ssh://git@repo_3.git", subdirectory = "lib_5", branch = "branch_2" }
lib_6 = { git = "ssh://git@repo_3.git", subdirectory = "lib_6", branch = "branch_2" }
lib_7 = { git = "ssh://git@repo_3.git", subdirectory = "lib_7", branch = "branch_2" }
```

Terminal:
```
PS E:\my_repo> uv sync -U --compile-bytecode
    Updated ssh://git@repo_1.git (eb3962c2f787c2b3720249e6d74dfe06f335fa4c)
    Updated ssh://git@repo_2.git (3a6fc4660888eb1a04ea85d9d03fd0c956685ec4)
    Updated ssh://git@repo_3.git (4844f61c4546abdd08abb624037c4948c2740c20)
    Updated ssh://git@repo_3.git (4844f61c4546abdd08abb624037c4948c2740c20)
    Updated ssh://git@repo_3.git (4844f61c4546abdd08abb624037c4948c2740c20)
    Updated ssh://git@repo_3.git (4844f61c4546abdd08abb624037c4948c2740c20)
    Updated ssh://git@repo_3.git (4844f61c4546abdd08abb624037c4948c2740c20)
    Updated ssh://git@repo_4.git (f6c189b3a5e7ddedb7fafc6093ceedfec9abc5c9)
Resolved 183 packages in 14.43s
```
After updating UV 0.6.2 to a higher version, an error occurred. 
UV started updating repository "repo_3" for all dependencies in pyproject.toml, which are taken from "repo_3"
I have 5 libraries from "repo_3" and during the update, UV receives the latest commit from "repo_3" several times

### Platform

Windows 10 x86_64

### Version

uv 0.6.4

### Python version

Python 3.12.2

---

_Label `bug` added by @Wintreist on 2025-03-04 14:49_

---

_Comment by @charliermarsh on 2025-03-04 14:51_

What is the error?

---

_Comment by @Wintreist on 2025-03-04 14:53_

> What is the error?

UV updates the same git repository several times
It makes unnecessary updates because it has already received the last commit.

---

_Comment by @charliermarsh on 2025-03-04 15:01_

That might be fine / correct. It doesn't mean it's pulling the repository. It just means it's ensuring that the commit is checked out locally.

---

_Comment by @Wintreist on 2025-03-04 15:03_

Understood, then I'm sorry. It wasn't obvious to me.

---

_Closed by @Wintreist on 2025-03-04 15:03_

---

_Comment by @charliermarsh on 2025-03-04 15:07_

No worries! Maybe think of it this way: we use a single clone of the repository under the hood. And in theory, you _could_ have multiple dependencies that use that repository, but with different commits. So when we go to build each dependency, we need to double-check that we have the right commit checked out. If it's already checked out, it's very cheap.

---
