```yaml
number: 10594
title: "Provide code action to ignore the diagnostic (via `noqa`)"
type: issue
state: closed
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2024-03-26T03:39:12Z
updated_at: 2024-05-12T21:39:48Z
url: https://github.com/astral-sh/ruff/issues/10594
synced_at: 2026-01-10T11:09:52Z
```

# Provide code action to ignore the diagnostic (via `noqa`)

---

_Issue opened by @dhruvmanila on 2024-03-26 03:39_

Provide "Disable for this line" code action to automatically add `noqa` comment for the selected diagnostic:

<img width="488" alt="Screenshot 2024-03-26 at 09 00 23" src="https://github.com/astral-sh/ruff/assets/67177269/1bcd8142-055f-468c-8774-57462b29c48e">

Reference implementation in `ruff-lsp`: https://github.com/astral-sh/ruff-lsp/blob/187d7790be0783b9ac41ce025a724cf389bf575c/ruff_lsp/server.py#L1007-L1069

---

_Label `server` added by @dhruvmanila on 2024-03-26 03:39_

---

_Assigned to @snowsignal by @snowsignal on 2024-03-26 05:00_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-10 23:36_

---

_Closed by @snowsignal on 2024-05-12 21:39_

---
