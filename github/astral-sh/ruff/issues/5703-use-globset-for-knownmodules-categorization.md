---
number: 5703
title: "Use `globset` for `KnownModules` categorization"
type: issue
state: open
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-07-12T04:34:43Z
updated_at: 2023-07-12T04:34:47Z
url: https://github.com/astral-sh/ruff/issues/5703
synced_at: 2026-01-07T13:12:15-06:00
---

# Use `globset` for `KnownModules` categorization

---

_Issue opened by @charliermarsh on 2023-07-12 04:34_

Rather than maintaining a vector of `Vec<(glob::Pattern, ImportSection)>`, in theory we could collapse the patterns into `Vec<(globset::GlobSet, ImportSection)>`. It's worth profiling to see whether this is impactful on real projects.

---

_Label `performance` added by @charliermarsh on 2023-07-12 04:34_

---
