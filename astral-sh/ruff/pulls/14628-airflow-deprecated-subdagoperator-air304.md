```yaml
number: 14628
title: "[airflow] deprecated subdagoperator (AIR304)"
type: pull_request
state: closed
author: abhishekbhakat
labels:
  - rule
  - preview
assignees: []
base: main
head: deprecated-subdag-operator
created_at: 2024-11-27T08:14:28Z
updated_at: 2024-12-10T15:47:58Z
url: https://github.com/astral-sh/ruff/pull/14628
synced_at: 2026-01-12T15:55:48Z
```

# [airflow] deprecated subdagoperator (AIR304)

---

_@abhishekbhakat_

Summary
Airflow 3.0 deprecates the SubDagOperator in favor of TaskGroups. SubDagOperator has been known to cause deadlocks and performance issues, and will be removed in Airflow 3.0. This rule detects usage of SubDagOperator to help users migrate their DAGs to the more reliable TaskGroup pattern.

Ref: #14626 

Test Plan
A test fixture has been included for the rule demonstrating both the deprecated SubDagOperator usage and the recommended TaskGroup alternative.


---

_Comment by @MichaReiser on 2024-11-27 08:18_

Same as for https://github.com/astral-sh/ruff/pull/14616 Let's first propose a set of rules that you plan of adding. I want to avoid having 50 or so airflow rules in ruff.

---

_Label `rule` added by @MichaReiser on 2024-11-27 08:18_

---

_Comment by @github-actions[bot] on 2024-11-27 08:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `preview` added by @MichaReiser on 2024-11-27 08:56_

---

_Comment by @MichaReiser on 2024-12-10 13:04_

@abhishekbhakat can we convert this PR back to draft state or close it if it's no longer needed according to the new airflow deprecation rules. It would help me to get a better sense for which PRs need my attention.

---

_Closed by @abhishekbhakat on 2024-12-10 15:47_

---
