```yaml
number: 5090
title: Binding range should always match the range of the bound identifier
type: issue
state: closed
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2023-06-14T16:37:51Z
updated_at: 2023-06-15T18:43:21Z
url: https://github.com/astral-sh/ruff/issues/5090
synced_at: 2026-01-12T15:54:45Z
```

# Binding range should always match the range of the bound identifier

---

_@charliermarsh_

For every `Binding`, we store a range -- but the exact contents of that range vary. E.g., for class definitions, it's the range of the entire class.

We should ensure that `Binding` always stores the exact range of the identifier.


---

_Label `core` added by @charliermarsh on 2023-06-14 16:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-14 16:37_

---

_Closed by @charliermarsh on 2023-06-15 18:43_

---
