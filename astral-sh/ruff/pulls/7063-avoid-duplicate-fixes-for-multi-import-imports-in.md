```yaml
number: 7063
title: Avoid duplicate fixes for multi-import imports in RUF017
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/symbols
created_at: 2023-09-02T11:32:43Z
updated_at: 2023-09-02T11:58:19Z
url: https://github.com/astral-sh/ruff/pull/7063
synced_at: 2026-01-12T15:55:23Z
```

# Avoid duplicate fixes for multi-import imports in RUF017

---

_@charliermarsh_

If a user has `import collections, functools, operator`, and we try to import from `functools` and `operator`, we end up adding two identical synthetic edits to preserve that import statement. We need to dedupe them.

Closes https://github.com/astral-sh/ruff/issues/7059.

---

_Label `bug` added by @charliermarsh on 2023-09-02 11:32_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-02 11:32_

---

_Comment by @github-actions[bot] on 2023-09-02 11:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-09-02 11:58_

---

_Closed by @charliermarsh on 2023-09-02 11:58_

---

_Branch deleted on 2023-09-02 11:58_

---
