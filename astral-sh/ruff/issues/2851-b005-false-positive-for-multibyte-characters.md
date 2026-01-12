```yaml
number: 2851
title: B005 false positive for multibyte characters
type: issue
state: closed
author: S-aiueo32
labels:
  - bug
assignees: []
created_at: 2023-02-13T11:38:40Z
updated_at: 2023-02-13T15:07:56Z
url: https://github.com/astral-sh/ruff/issues/2851
synced_at: 2026-01-12T15:54:43Z
```

# B005 false positive for multibyte characters

---

_@S-aiueo32_

```
"".strip("a")  # negative
"".strip("あ")  # positive
```

---

_Label `bug` added by @charliermarsh on 2023-02-13 13:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-13 14:40_

---

_Closed by @charliermarsh on 2023-02-13 15:07_

---
