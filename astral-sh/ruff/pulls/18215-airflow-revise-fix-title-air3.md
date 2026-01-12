```yaml
number: 18215
title: "[`airflow`] Revise fix title `AIR3`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: revise-AIR3-fix-title
created_at: 2025-05-20T07:50:13Z
updated_at: 2025-05-26T09:31:49Z
url: https://github.com/astral-sh/ruff/pull/18215
synced_at: 2026-01-12T15:56:14Z
```

# [`airflow`] Revise fix title `AIR3`

---

_@Lee-W_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Originally, we use `Use {module}.{name} instead` as fix title. But it could be confusing in some cases like `airflow.providers.amazon.aws.auth_manager.avp.entitiesAvpEntities.ASSET`. It's not wrong. But user won't be able to do 

```python
from airflow.providers.amazon.aws.auth_manager.avp.entities.AvpEntities import ASSET
```

the following is what actually works

```python
from airflow.providers.amazon.aws.auth_manager.avp.entities import AvpEntities
AvpEntities.ASSET
```


## Test Plan

The message has been updated accordingly
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-20 07:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-05-24 15:53_

---

_Label `preview` added by @ntBre on 2025-05-24 15:53_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:75 on 2025-05-24 16:09_

Should this be `{name} from {module}` too?

---

_@ntBre approved on 2025-05-24 16:10_

Thanks, this makes sense to me. Just one question about a potentially inconsistent message.

---

_@Lee-W reviewed on 2025-05-25 02:47_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:75 on 2025-05-25 02:47_

Right... I revised all the messages for a few times and missed this one. Thanks! Just updated it

---

_Merged by @MichaReiser on 2025-05-26 09:31_

---

_Closed by @MichaReiser on 2025-05-26 09:31_

---
