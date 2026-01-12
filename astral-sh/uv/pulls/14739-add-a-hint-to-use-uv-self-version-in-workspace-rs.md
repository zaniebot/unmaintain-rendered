```yaml
number: 14739
title: Add a hint to use uv self version in workspace.rs
type: pull_request
state: closed
author: farooqameen
labels: []
assignees: []
base: main
head: main
created_at: 2025-07-18T18:22:19Z
updated_at: 2025-07-18T18:58:00Z
url: https://github.com/astral-sh/uv/pull/14739
synced_at: 2026-01-12T16:11:22Z
```

# Add a hint to use uv self version in workspace.rs

---

_@farooqameen_

## Summary
Fixes #14730 by adding the required hint in workspace.rs

## Test Plan

"cargo nextest run -p workspace-uv" passed all checks
"uv version" returned the appropriate hint when pyproject.toml was missing

This is my first pull request - if additional details are needed, please let me know!

---

_Comment by @farooqameen on 2025-07-18 18:57_

Will study some more - thank you!

---

_Closed by @farooqameen on 2025-07-18 18:57_

---
