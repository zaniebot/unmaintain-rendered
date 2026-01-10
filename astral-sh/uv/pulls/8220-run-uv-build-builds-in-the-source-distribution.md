```yaml
number: 8220
title: "Run `uv build` builds in the source distribution bucket"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/git-root-for-dist
created_at: 2024-10-15T14:36:00Z
updated_at: 2024-10-15T17:29:52Z
url: https://github.com/astral-sh/uv/pull/8220
synced_at: 2026-01-10T12:54:04Z
```

# Run `uv build` builds in the source distribution bucket

---

_Pull request opened by @konstin on 2024-10-15 14:36_

When building a source distribution to a wheels, we perform the build inside a temporary directory inside the output directory. By default, the output directory is `dist/` in the repository root. This temp dir placement allows us to move the final wheel to the output directory instead of copying it (a temp dir might be on another device, which means we need to copy instead of moving).

Some build backends such as hatchling traverse upwards from the current directory (the source dist build location) looking for gitignore files to consider. By adding a gitignore in `dist/` with `*`, we caused hatchling to ignore all files in our temporary build directory below it, causing empty wheels. To prevent this, we add a `.git` file as a phony git root. We are already using this trick successfully in the cache. Hatchling sees this `.git` file, considers it a boundary and does not traverse up to `dist/.gitignore`.

Fixes #8200

---

_Label `bug` added by @konstin on 2024-10-15 14:36_

---

_Review requested from @charliermarsh by @konstin on 2024-10-15 14:36_

---

_Review requested from @zanieb by @konstin on 2024-10-15 14:36_

---

_@charliermarsh approved on 2024-10-15 14:55_

---

_Comment by @charliermarsh on 2024-10-15 15:25_

Could we instead perform the builds in `CacheBucket::Builds`? I think it exists for this purpose.

---

_Renamed from "Add a fake git root for `dist`" to "Run `uv build` builds in the source distribution bucket" by @charliermarsh on 2024-10-15 17:21_

---

_@zanieb approved on 2024-10-15 17:27_

---

_Merged by @charliermarsh on 2024-10-15 17:29_

---

_Closed by @charliermarsh on 2024-10-15 17:29_

---

_Branch deleted on 2024-10-15 17:29_

---
