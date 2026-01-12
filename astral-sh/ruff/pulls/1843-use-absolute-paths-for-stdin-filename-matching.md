```yaml
number: 1843
title: Use absolute paths for --stdin-filename matching
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/norm
created_at: 2023-01-13T01:49:00Z
updated_at: 2023-01-13T02:01:06Z
url: https://github.com/astral-sh/ruff/pull/1843
synced_at: 2026-01-12T05:36:32Z
```

# Use absolute paths for --stdin-filename matching

---

_Pull request opened by @charliermarsh on 2023-01-13 01:49_

Non-basename glob matches (e.g., for `--per-file-ignores`) assume that the path has been converted to an absolute path. (We do this for filenames as part of the directory traversal.) For filenames passed via stdin, though, we're missing this conversion. So `--per-file-ignores` that rely on the _basename_ worked as expected, but directory paths did not.

Closes #1840.

---

_Merged by @charliermarsh on 2023-01-13 02:01_

---

_Closed by @charliermarsh on 2023-01-13 02:01_

---

_Branch deleted on 2023-01-13 02:01_

---
