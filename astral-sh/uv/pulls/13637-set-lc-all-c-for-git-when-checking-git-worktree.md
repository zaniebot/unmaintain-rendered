```yaml
number: 13637
title: "Set `LC_ALL=C` for git when checking git worktree"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: git-locale
created_at: 2025-05-24T14:09:20Z
updated_at: 2025-05-24T15:16:33Z
url: https://github.com/astral-sh/uv/pull/13637
synced_at: 2026-01-10T11:10:42Z
```

# Set `LC_ALL=C` for git when checking git worktree

---

_Pull request opened by @j178 on 2025-05-24 14:09_

## Summary

Closes #13612

We check if the git error message includes `not a git repository` to figure out if the path isn't a Git repo or if Git's broken. This PR sets `LC_ALL=C` when invoking `git rev-parse --is-inside-work-tree` so that the error message doesn’t change based on the user’s locale settings.


---

_Merged by @charliermarsh on 2025-05-24 15:05_

---

_Closed by @charliermarsh on 2025-05-24 15:05_

---

_Comment by @charliermarsh on 2025-05-24 15:05_

Thanks!

---

_Label `bug` added by @charliermarsh on 2025-05-24 15:05_

---

_Branch deleted on 2025-05-24 15:16_

---
