```yaml
number: 15159
title: "[airflow]: extend names moved from core to provider (AIR303)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR303
created_at: 2024-12-27T00:29:25Z
updated_at: 2024-12-27T17:04:03Z
url: https://github.com/astral-sh/ruff/pull/15159
synced_at: 2026-01-10T20:42:27Z
```

# [airflow]: extend names moved from core to provider (AIR303)

---

_Pull request opened by @Lee-W on 2024-12-27 00:29_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Many core Airflow features have been deprecated and moved to Airflow Providers since users might need to install an additional package (e.g., `apache-airflow-provider-fab==1.0.0`); a separate rule (AIR303) is created for this.

* apache-airflow-providers-common-sql >= 1.0.0
    * `airflow.hooks.dbapi.ConnectorProtocol` → `airflow.providers.common.sql.hooks.sql.ConnectorProtocol`
    * `airflow.hooks.dbapi.DbApiHook` → `airflow.providers.common.sql.hooks.sql.DbApiHook`
* apache-airflow-providers-cncf-kubernetes >= 7.4.0
    * `airflow.executors.kubernetes_executor_types.ALL_NAMESPACES` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor_types.ALL_NAMESPACES`
    * `airflow.executors.kubernetes_executor_types.POD_EXECUTOR_DONE_KEY` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor_types.POD_EXECUTOR_DONE_KEY`
* apache-airflow-providers-celery >= 3.3.0
    * `airflow.executors.celery_executor.app` → `airflow.providers.celery.executors.celery_executor_utils.app`
    * `airflow.config_templates.default_celery.DEFAULT_CELERY_CONFIG` → `airflow.providers.celery.executors.default_celery.DEFAULT_CELERY_CONFIG`
    * `airflow.executors.celery_kubernetes_executor.CeleryKubernetesExecutor` → `airflow.providers.celery.executors.celery_kubernetes_executor`
    * `airflow.executors.celery_executor.CeleryExecutor` → `airflow.providers.celery.executors.celery_executor.CeleryExecutor`
* apache-airflow-providers-apache-hive >= 1.0.0
    * `airflow.hooks.hive_hooks.HIVE_QUEUE_PRIORITIES` → `airflow.providers.apache.hive.hooks.hive.HIVE_QUEUE_PRIORITIES`
* apache-airflow-providers-apache-hive >= 5.1.0
    * `airflow.macros.hive.closest_ds_partition` → `airflow.providers.apache.hive.macros.hive.closest_ds_partition`
     `airflow.executors.kubernetes_executor.KubernetesExecutor` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor.KubernetesE
* apache-airflow-providers-cncf-kubernetes >= 7.4.0
    * `airflow.executors.kubernetes_executor_types.ALL_NAMESPACES` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor_types.ALL
    * `airflow.executors.kubernetes_executor_types.POD_EXECUTOR_DONE_KEY` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor_ty
    * `airflow.executors.kubernetes_executor_utils.AirflowKubernetesScheduler` → `airflow.providers.cncf.kubernetes.executors.kubernetes_execut
    * `airflow.executors.kubernetes_executor_utils.KubernetesJobWatcher` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor_uti
    * `airflow.executors.kubernetes_executor_utils.ResourceVersion` → `airflow.providers.cncf.kubernetes.executors.kubernetes_executor_utils.Re
    * `airflow.executors.local_kubernetes_executor.LocalKubernetesExecutor` → `airflow.providers.cncf.kubernetes.executors.LocalKubernetesExecu `airflow.macros.hive.max_partition` → `airflow.providers.apache.hive.macros.hive.max_partition`
* apache-airflow-providers-daskexecutor >= 1.0.0
    * `airflow.executors.dask_executor.DaskExecutor` → `airflow.providers.daskexecutor.executors.dask_executor.DaskExecutor`


## Test Plan

<!-- How was it tested? -->

A test fixture has been included for the rule.

---

_Comment by @github-actions[bot] on 2024-12-27 00:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2024-12-27 17:03_

---

_Label `preview` added by @MichaReiser on 2024-12-27 17:03_

---

_@MichaReiser approved on 2024-12-27 17:03_

Nice

---

_Merged by @MichaReiser on 2024-12-27 17:04_

---

_Closed by @MichaReiser on 2024-12-27 17:04_

---
