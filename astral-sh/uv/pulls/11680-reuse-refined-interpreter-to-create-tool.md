```yaml
number: 11680
title: Reuse refined interpreter to create tool environment
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/use
created_at: 2025-02-20T21:56:51Z
updated_at: 2025-02-20T22:05:04Z
url: https://github.com/astral-sh/uv/pull/11680
synced_at: 2026-01-10T11:10:38Z
```

# Reuse refined interpreter to create tool environment

---

_Pull request opened by @charliermarsh on 2025-02-20 21:56_

## Summary

As-is, we used the refined interpreter to _resolve_, but we then created the tool environment with the "old" interpreter. So we risked running (e.g.) code that requires Python 3.12 in a Python 3.10 environment. We need to propagate the updated interpreter.

This is fairly hard to test, because it requires an environment in which we're able to download new interpreters.

Closes https://github.com/astral-sh/uv/issues/11678#issuecomment-2672659074.


---

_Label `bug` added by @charliermarsh on 2025-02-20 21:56_

---

_Merged by @charliermarsh on 2025-02-20 22:05_

---

_Closed by @charliermarsh on 2025-02-20 22:05_

---

_Branch deleted on 2025-02-20 22:05_

---
