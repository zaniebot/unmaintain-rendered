```yaml
number: 20551
title: "[`airflow`] Add warning to `airflow.datasets.DatasetEvent` usage (`AIR301`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
assignees: []
merged: true
base: main
head: dataset-event
created_at: 2025-09-24T13:54:35Z
updated_at: 2025-10-16T02:59:09Z
url: https://github.com/astral-sh/ruff/pull/20551
synced_at: 2026-01-12T15:57:04Z
```

# [`airflow`] Add warning to `airflow.datasets.DatasetEvent` usage (`AIR301`)

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

`airflow.datasets.DatasetEvent` has been removed in 3 but `AssetEvent` might be added in the future

## Test Plan

<!-- How was it tested? -->

update the test fixture and reorg in the second commit


---

_Renamed from "feat(AIR301): add airflow.datasets.DatasetEvent" to "[airflow] add warning to `airflow.datasets.DatasetEvent` usage (AIR301)" by @Lee-W on 2025-09-24 13:56_

---

_Comment by @github-actions[bot] on 2025-09-24 14:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:659 on 2025-09-25 20:30_

nit: I think it might sound a little better to use `DatasetEvent` or even `airflow.datasets.DatasetEvent` in the help message instead of starting with `It`:


```suggestion
            "`DatasetEvent` has been made private in Airflow 3. An AssetEvent type will be added to the apache-airflow-task-sdk in a future version.",
```

---

_@ntBre reviewed on 2025-09-25 20:34_

Thanks! Just one nit on the phrasing. More generally, is this an actionable lint for users? It's not really clear (to me at least) how they should respond if the replacement isn't available yet. A few options that come to mind:
- Don't mention the future replacement
- Mention an alternative that can be used now
- Wait on adding this lint until the replacement is available

---

_Label `rule` added by @ntBre on 2025-09-25 20:36_

---

_Comment by @Lee-W on 2025-09-25 23:48_

We can suggest users to use `dict[str, Any]` for the time being. and then add the replacement at a later point. WDYT? 

I'm good with holding it as well :) it's not that urgent

---

_Comment by @ntBre on 2025-09-29 21:19_

Sure, `dict[str, Any]` sounds good to me! Or we can hold off for now, whichever you prefer :) 

---

_Comment by @Lee-W on 2025-10-06 07:21_

Just updated it! Merging it early would be cool, but I'm fine if we decide to wait. We should be able to have it in the next airflow task SDK release.

---

_Comment by @ntBre on 2025-10-15 16:19_

Sounds good, I'm happy to merge this and update it again later :) Thank you!

---

_Renamed from "[airflow] add warning to `airflow.datasets.DatasetEvent` usage (AIR301)" to "[`airflow`] Add warning to `airflow.datasets.DatasetEvent` usage (`AIR301`)" by @ntBre on 2025-10-15 16:19_

---

_Merged by @ntBre on 2025-10-15 16:19_

---

_Closed by @ntBre on 2025-10-15 16:19_

---

_Branch deleted on 2025-10-16 02:59_

---
