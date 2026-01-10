```yaml
number: 3732
title: "MISSING_HEADER_RE doesn't cover clang (or macos?) cases"
type: issue
state: closed
author: palfrey
labels:
  - good first issue
  - error messages
assignees: []
created_at: 2024-05-22T09:43:18Z
updated_at: 2024-05-22T18:40:50Z
url: https://github.com/astral-sh/uv/issues/3732
synced_at: 2026-01-10T05:31:37Z
```

# MISSING_HEADER_RE doesn't cover clang (or macos?) cases

---

_Issue opened by @palfrey on 2024-05-22 09:43_

In the pygraphviz case for example, the text is `pygraphviz/graphviz_wrap.c:3020:10: fatal error: 'graphviz/cgraph.h' file not found` which doesn't match `MISSING_HEADER_RE` in https://github.com/astral-sh/uv/blob/main/crates/uv-build/src/lib.rs#L37

---

_Label `error messages` added by @charliermarsh on 2024-05-22 13:18_

---

_Label `good first issue` added by @charliermarsh on 2024-05-22 13:18_

---

_Closed by @charliermarsh on 2024-05-22 18:40_

---
