```yaml
number: 12240
title: False positive S113 for httpx.AsyncClient
type: issue
state: closed
author: dybi
labels: []
assignees: []
created_at: 2024-07-08T09:22:00Z
updated_at: 2024-07-08T09:25:09Z
url: https://github.com/astral-sh/ruff/issues/12240
synced_at: 2026-01-10T11:09:54Z
```

# False positive S113 for httpx.AsyncClient

---

_Issue opened by @dybi on 2024-07-08 09:22_

`S113` is reported by `ruff` for `httpx.AsyncClient()` (https://docs.astral.sh/ruff/rules/request-without-timeout/)
whereas `httpx` works differntly than `requests` in this area and there is a finite default: https://github.com/encode/httpx/blob/db9072f998b53ff66d50778bf5edee8e2cc8ede1/httpx/_client.py#L1390

Minimal app do reproduce:
```python
import httpx

client = httpx.AsyncClient()
```

`ruff` version = `0.5.1.`


---

_Comment by @MichaReiser on 2024-07-08 09:24_

Yes sorry, that was an oversight on our end and thanks for reporting! I think this is already fixed and will go out in our next release https://github.com/astral-sh/ruff/pull/12213

---

_Closed by @MichaReiser on 2024-07-08 09:24_

---
