```yaml
number: 8665
title: Support transitive dependencies in Git workspaces
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-git-workspaces
created_at: 2024-10-29T16:07:19Z
updated_at: 2024-10-30T19:12:24Z
url: https://github.com/astral-sh/uv/pull/8665
synced_at: 2026-01-12T16:08:26Z
```

# Support transitive dependencies in Git workspaces

---

_@konstin_

When resolving workspace dependencies (from one workspace member to another) from a workspace that's in git, we need to emit these transitive dependencies as git dependencies, not path dependencies as all other workspace deps. This fixes a bug where we would treat them as path dependencies inside the checkout directory, leading either to clashes (between a local path and another direct git dependency) or invalid lockfiles (referencing the checkout dir in the lockfile when we should be referencing the git repo).

Fixes #8087
Fixes #4920
Fixes #3936 since we needed that information anyway


---

_Label `bug` added by @konstin on 2024-10-29 16:07_

---

_@charliermarsh approved on 2024-10-30 19:05_

---

_Comment by @charliermarsh on 2024-10-30 19:06_

Looks good to me. There's one failure on Windows, but I can look at fixing it in a separate PR.

---

_Renamed from "Fix git workspace transitive dependencies" to "Support transitive dependencies in Git workspaces" by @charliermarsh on 2024-10-30 19:11_

---

_Merged by @charliermarsh on 2024-10-30 19:12_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---

_Branch deleted on 2024-10-30 19:12_

---
