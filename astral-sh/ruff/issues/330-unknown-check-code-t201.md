```yaml
number: 330
title: "Unknown check code: T201"
type: issue
state: closed
author: YangtseSu
labels: []
assignees: []
created_at: 2022-10-05T15:54:51Z
updated_at: 2022-10-05T15:59:25Z
url: https://github.com/astral-sh/ruff/issues/330
synced_at: 2026-01-12T15:54:40Z
```

# Unknown check code: T201

---

_@YangtseSu_

```
ruff --select T201 ./T201.py
error: Invalid value "T201" for '--select <SELECT>': Unknown check code: T201
```

---

_Closed by @charliermarsh on 2022-10-05 15:58_

---

_Comment by @charliermarsh on 2022-10-05 15:58_

Thank you! Cutting a new release now.

---

_Comment by @charliermarsh on 2022-10-05 15:59_

Will be part of `v0.0.56` (building now).

---
