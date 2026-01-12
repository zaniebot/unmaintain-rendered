```yaml
number: 2503
title: "Remove a result wrapper from `linter.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/result
created_at: 2023-02-02T23:39:31Z
updated_at: 2023-02-02T23:47:47Z
url: https://github.com/astral-sh/ruff/pull/2503
synced_at: 2026-01-12T15:55:08Z
```

# Remove a result wrapper from `linter.rs`

---

_@charliermarsh_

We should only be passing valid filenames to this file (since we only pass _collected_ filenames to this file, which have already gone through `extract_path_names`). Changing this to an irrecoverable error lets us drop a lot of ceremony elsewhere.

---

_Renamed from "Remove a result wrapper from linter.rs" to "Remove a result wrapper from `linter.rs`" by @charliermarsh on 2023-02-02 23:40_

---

_Merged by @charliermarsh on 2023-02-02 23:47_

---

_Closed by @charliermarsh on 2023-02-02 23:47_

---

_Branch deleted on 2023-02-02 23:47_

---
