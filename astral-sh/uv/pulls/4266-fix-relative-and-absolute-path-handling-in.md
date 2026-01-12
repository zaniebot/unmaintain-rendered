```yaml
number: 4266
title: Fix relative and absolute path handling in lockfiles
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/relative-path-handling-fix
created_at: 2024-06-12T10:46:44Z
updated_at: 2024-06-13T15:51:09Z
url: https://github.com/astral-sh/uv/pull/4266
synced_at: 2026-01-12T16:06:07Z
```

# Fix relative and absolute path handling in lockfiles

---

_@konstin_

Previously, `b` in the test case would have been incorrectly locked to the path of `a`. I've moved `relative_to` into uv-fs since it's now used in two different places.

Previously failing lockfile when `a/pyproject.toml` and `a/b/pyproject.toml` exist (not in a workspace) and `a` was depending on `b`:

```toml
version = 1
requires-python = ">=3.11, <3.13"

[[distribution]]
name = "b"
version = "0.1.0"
source = "directory+/home/konsti/projects/uv/a"
sdist = { path = "/home/konsti/projects/uv/a" }

[[distribution]]
name = "black"
version = "0.1.0"
source = "editable+."
sdist = { path = "." }

[[distribution.dependencies]]
name = "b"
version = "0.1.0"
source = "directory+/home/konsti/projects/uv/a"
```

---

_Label `bug` added by @konstin on 2024-06-12 10:46_

---

_Label `preview` added by @konstin on 2024-06-12 10:46_

---

_@BurntSushi approved on 2024-06-12 12:20_

---

_Merged by @BurntSushi on 2024-06-13 15:51_

---

_Closed by @BurntSushi on 2024-06-13 15:51_

---

_Branch deleted on 2024-06-13 15:51_

---
