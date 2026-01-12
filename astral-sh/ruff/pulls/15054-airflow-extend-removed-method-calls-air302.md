```yaml
number: 15054
title: "[airflow]: extend removed method calls (AIR302)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR302-with-method-call
created_at: 2024-12-19T09:20:16Z
updated_at: 2025-01-23T08:50:09Z
url: https://github.com/astral-sh/ruff/pull/15054
synced_at: 2026-01-12T15:55:50Z
```

# [airflow]: extend removed method calls (AIR302)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Airflow 3.0 removes various deprecated functions, members, modules, and other values. They have been deprecated in 2.x, but the removal causes incompatibilities that we want to detect. This PR deprecates the following names and add a function for removed methods

* `airflow.datasets.manager.DatasetManager.register_dataset_change` → `airflow.assets.manager.AssetManager.register_asset_change`
* `airflow.datasets.manager.DatasetManager.create_datasets` → `airflow.assets.manager.AssetManager.create_assets`
* `airflow.datasets.manager.DatasetManager.notify_dataset_created` → `airflow.assets.manager.AssetManager.notify_asset_created`
* `airflow.datasets.manager.DatasetManager.notify_dataset_changed` → `airflow.assets.manager.AssetManager.notify_asset_changed`
* `airflow.datasets.manager.DatasetManager.notify_dataset_alias_created` → `airflow.assets.manager.AssetManager.notify_asset_alias_created`
* `airflow.providers.amazon.auth_manager.aws_auth_manager.AwsAuthManager.is_authorized_dataset` → `airflow.providers.amazon.auth_manager.aws_auth_manager.AwsAuthManager.is_authorized_asset`
* `airflow.lineage.hook.HookLineageCollector.create_dataset` → `airflow.lineage.hook.HookLineageCollector.create_asset`
* `airflow.lineage.hook.HookLineageCollector.add_input_dataset` → `airflow.lineage.hook.HookLineageCollector.add_input_asset`
* `airflow.lineage.hook.HookLineageCollector.add_output_dataset` → `airflow.lineage.hook.HookLineageCollector.dd_output_asset`
* `airflow.lineage.hook.HookLineageCollector.collected_datasets` → `airflow.lineage.hook.HookLineageCollector.collected_assets`
* `airflow.providers_manager.ProvidersManager.initialize_providers_dataset_uri_resources` → `airflow.providers_manager.ProvidersManager.initialize_providers_asset_uri_resources`

## Test Plan

<!-- How was it tested? -->
A test fixture is included in the PR.

---

_Renamed from "feat(AIR302): add the following rules for method name changes" to "[airflow]: extend removed method calls (AIR302)" by @Lee-W on 2024-12-19 09:22_

---

_Comment by @github-actions[bot] on 2024-12-19 09:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2024-12-19 12:09_

---

_Label `preview` added by @MichaReiser on 2024-12-19 12:09_

---

_@MichaReiser reviewed on 2024-12-19 12:12_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:718 on 2024-12-19 12:12_

Can we skip calling `removed_method` if the `resolved_qualified_name` call was successful or does it need both?

---

_@Lee-W reviewed on 2024-12-19 13:24_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:718 on 2024-12-19 13:24_

I think both are needed.



---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:718 on 2024-12-19 13:56_

e.g.,

```python
from airflow.datasets.manager import DatasetManager

d = DatasetManager()
d.register_dataset_change()
```

needs 2 warnings

1. import error
2. method call error

---

_@Lee-W reviewed on 2024-12-19 13:56_

---

_@Lee-W reviewed on 2024-12-19 13:58_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:170 on 2024-12-19 13:58_

Clippy suggest me to change it to `&Expr`, but it failed after I changed it. Would like to see whether there's suggestion on this one. Thanks!

---

_@Lee-W reviewed on 2024-12-19 13:58_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:158 on 2024-12-19 13:58_

I also tried to remove `Option` as clippy suggested, but failed as well 😢 

---

_Merged by @MichaReiser on 2024-12-20 07:30_

---

_Closed by @MichaReiser on 2024-12-20 07:30_

---

_Branch deleted on 2025-01-23 08:50_

---
