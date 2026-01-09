---
number: 17103
title: "[airflow] Autofix is suggesting to replace with the same symbol"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - preview
assignees: []
created_at: 2025-03-31T22:11:22Z
updated_at: 2025-04-07T13:45:58Z
url: https://github.com/astral-sh/ruff/issues/17103
synced_at: 2026-01-07T13:12:16-06:00
---

# [airflow] Autofix is suggesting to replace with the same symbol

---

_Issue opened by @dhruvmanila on 2025-03-31 22:11_

This looks like a bug:
```
AIR302 `airflow.sensors.external_task.ExternalTaskSensor` is removed in Airflow 3.0
[...]
= help: Use `airflow.sensors.external_task.ExternalTaskSensor` instead
```

It's suggesting replacing with itself

_Originally posted by @pbhuss in https://github.com/astral-sh/ruff/issues/16014#issuecomment-2767505991_
            

---

_Label `bug` added by @dhruvmanila on 2025-03-31 22:11_

---

_Label `preview` added by @dhruvmanila on 2025-03-31 22:11_

---

_Comment by @dhruvmanila on 2025-03-31 22:12_

cc @Lee-W 

---

_Comment by @Lee-W on 2025-04-01 08:14_

This will be addressed in the reorg PR. This is later moved to provider as well. You can just assign this one to me. Thanks!

---

_Assigned to @Lee-W by @MichaReiser on 2025-04-01 11:09_

---

_Referenced in [astral-sh/ruff#17123](../../astral-sh/ruff/pulls/17123.md) on 2025-04-02 02:12_

---

_Closed by @ntBre on 2025-04-07 13:45_

---

_Closed by @ntBre on 2025-04-07 13:45_

---
