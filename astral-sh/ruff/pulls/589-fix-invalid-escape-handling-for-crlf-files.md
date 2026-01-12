```yaml
number: 589
title: Fix invalid escape handling for CRLF files
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/windows
created_at: 2022-11-04T18:25:56Z
updated_at: 2022-11-04T18:26:40Z
url: https://github.com/astral-sh/ruff/pull/589
synced_at: 2026-01-12T15:55:05Z
```

# Fix invalid escape handling for CRLF files

---

_@charliermarsh_

This PR reimplements `invalid_escape_sequence` to properly handle files with CRLF newlines (as on Windows). The implementation is a bit slower but overall simpler.

Resolves #581.


---

_Merged by @charliermarsh on 2022-11-04 18:26_

---

_Closed by @charliermarsh on 2022-11-04 18:26_

---

_Branch deleted on 2022-11-04 18:26_

---
