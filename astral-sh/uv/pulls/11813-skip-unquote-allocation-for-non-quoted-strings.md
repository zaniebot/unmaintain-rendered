```yaml
number: 11813
title: Skip unquote allocation for non-quoted strings
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/mem
created_at: 2025-02-26T21:47:56Z
updated_at: 2025-02-26T21:56:32Z
url: https://github.com/astral-sh/uv/pull/11813
synced_at: 2026-01-12T16:10:00Z
```

# Skip unquote allocation for non-quoted strings

---

_@charliermarsh_

## Summary

Small optimization: no need to unquote if there aren't any quote characters.


---

_Label `performance` added by @charliermarsh on 2025-02-26 21:47_

---

_@zanieb approved on 2025-02-26 21:48_

---

_Merged by @charliermarsh on 2025-02-26 21:56_

---

_Closed by @charliermarsh on 2025-02-26 21:56_

---

_Branch deleted on 2025-02-26 21:56_

---
