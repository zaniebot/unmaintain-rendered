```yaml
number: 2072
title: ICN001 check from imports that have no alias
type: pull_request
state: merged
author: Zeddicus414
labels: []
assignees: []
merged: true
base: main
head: ICN001-check-from-imports-no-alias
created_at: 2023-01-21T22:03:43Z
updated_at: 2023-01-22T04:14:49Z
url: https://github.com/astral-sh/ruff/pull/2072
synced_at: 2026-01-12T04:52:00Z
```

# ICN001 check from imports that have no alias

---

_Pull request opened by @Zeddicus414 on 2023-01-21 22:03_

Add tests.

Ensure that these cases are caught by ICN001:
```python
from xml.dom import minidom
from xml.dom.minidom import parseString
```

with config:
```toml
[tool.ruff.flake8-import-conventions.extend-aliases]
"dask.dataframe" = "dd"
"xml.dom.minidom" = "md"
"xml.dom.minidom.parseString" = "pstr"
```

---

_Renamed from "Icn001 check from imports no alias" to "ICN001 check from imports that have no alias" by @Zeddicus414 on 2023-01-21 22:04_

---

_Merged by @charliermarsh on 2023-01-21 22:47_

---

_Closed by @charliermarsh on 2023-01-21 22:47_

---

_Branch deleted on 2023-01-22 04:14_

---
