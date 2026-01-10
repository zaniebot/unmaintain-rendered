```yaml
number: 12039
title: "Add `asyncio` and `anyio` support to `ASYNC1XX`"
type: issue
state: closed
author: MichaReiser
labels:
  - rule
assignees: []
created_at: 2024-06-26T08:02:50Z
updated_at: 2024-07-10T07:43:13Z
url: https://github.com/astral-sh/ruff/issues/12039
synced_at: 2026-01-10T11:09:54Z
```

# Add `asyncio` and `anyio` support to `ASYNC1XX`

---

_Issue opened by @MichaReiser on 2024-06-26 08:02_

The `ASYNC1xx` rules used to be `trio` specific rules, but the upstream `flake8-async` rules now also cover `asyncio` and `anyio` (see https://github.com/astral-sh/ruff/pull/10416 for when we recoded the `trio` rules to `flake8-async`). 

We should update

* `ASYNC100`
* `ASYNC109`
* `ASYNC110`
* `ASYNC115`

To also cover `anyio` and `asyncio` to match the upstream rule's behavior. 



---

_Label `rule` added by @MichaReiser on 2024-06-26 08:02_

---

_Closed by @MichaReiser on 2024-07-10 07:43_

---
