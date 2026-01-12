```yaml
number: 17673
title: "[airflow] fix typos `AIR312`"
type: pull_request
state: merged
author: Jie211
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix_airflow_help_msg
created_at: 2025-04-28T04:59:20Z
updated_at: 2025-04-28T06:31:41Z
url: https://github.com/astral-sh/ruff/pull/17673
synced_at: 2026-01-12T15:56:03Z
```

# [airflow] fix typos `AIR312`

---

_@Jie211_

## Summary

Fix incorrect package path in apache-airflow-providers-standard:
There is no time directory in the package structure.

Before:
```
airflow.providers.standard.time.operators.datetime.BranchDateTimeOperator
```

After:
```
airflow.providers.standard.operators.datetime.BranchDateTimeOperator
```

This change aligns with the actual source code structure in apache-airflow-providers-standard==1.0.0:
https://github.com/apache/airflow/tree/providers-standard/1.0.0/providers/standard/src/airflow/providers/standard


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

test fixture has been updated


---

_Comment by @MichaReiser on 2025-04-28 06:17_

CC: @Lee-W 

---

_Label `rule` added by @MichaReiser on 2025-04-28 06:17_

---

_Label `preview` added by @MichaReiser on 2025-04-28 06:17_

---

_Comment by @github-actions[bot] on 2025-04-28 06:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Lee-W reviewed on 2025-04-28 06:27_

LGTM. Thanks for pointing out!

---

_Merged by @MichaReiser on 2025-04-28 06:31_

---

_Closed by @MichaReiser on 2025-04-28 06:31_

---
