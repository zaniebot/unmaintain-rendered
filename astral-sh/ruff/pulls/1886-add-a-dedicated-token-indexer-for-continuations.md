```yaml
number: 1886
title: Add a dedicated token indexer for continuations and comments
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/indexer
created_at: 2023-01-15T06:49:23Z
updated_at: 2023-01-15T06:57:32Z
url: https://github.com/astral-sh/ruff/pull/1886
synced_at: 2026-01-12T05:36:32Z
```

# Add a dedicated token indexer for continuations and comments

---

_Pull request opened by @charliermarsh on 2023-01-15 06:49_

The primary motivation is that we can now robustly detect `\` continuations due to the addition of `Tok::NonLogicalNewline`. This PR generalizes the approach we took to comments (track all lines that contain any comments), and applies it to continuations too.

---

_Merged by @charliermarsh on 2023-01-15 06:57_

---

_Closed by @charliermarsh on 2023-01-15 06:57_

---

_Branch deleted on 2023-01-15 06:57_

---
