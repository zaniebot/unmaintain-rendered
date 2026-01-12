```yaml
number: 15216
title: "[`airflow`] Extend names moved from core to provider (`AIR303`)"
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
created_at: 2025-01-02T02:59:24Z
updated_at: 2025-01-02T12:43:10Z
url: https://github.com/astral-sh/ruff/pull/15216
synced_at: 2026-01-12T15:55:50Z
```

# [`airflow`] Extend names moved from core to provider (`AIR303`)

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

Many core Airflow features have been deprecated and moved to Airflow Providers since users might need to install an additional package (e.g., `apache-airflow-provider-fab==1.0.0`); a separate rule (AIR303) is created for this.

* `airflow.kubernetes.kubernetes_helper_functions.add_pod_suffix` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.add_pod_suffix`
* `airflow.kubernetes.kubernetes_helper_functions.annotations_for_logging_task_metadata` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.annotations_for_logging_task_metadata`
* `airflow.kubernetes.kubernetes_helper_functions.annotations_to_key` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.annotations_to_key`
* `airflow.kubernetes.kubernetes_helper_functions.create_pod_id` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.create_pod_id`
* `airflow.kubernetes.kubernetes_helper_functions.get_logs_task_metadata` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.get_logs_task_metadata`
* `airflow.kubernetes.kubernetes_helper_functions.rand_str` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.rand_str`
* `airflow.kubernetes.pod.Port` → `kubernetes.client.models.V1ContainerPort`
* `airflow.kubernetes.pod.Resources` → `kubernetes.client.models.V1ResourceRequirements`
* `airflow.kubernetes.pod_launcher.PodLauncher` → `airflow.providers.cncf.kubernetes.pod_launcher.PodLauncher`
* `airflow.kubernetes.pod_launcher.PodStatus` → `airflow.providers.cncf.kubernetes.pod_launcher.PodStatus`
* `airflow.kubernetes.pod_launcher_deprecated.PodLauncher` → `airflow.providers.cncf.kubernetes.pod_launcher_deprecated.PodLauncher`
* `airflow.kubernetes.pod_launcher_deprecated.PodStatus` → `airflow.providers.cncf.kubernetes.pod_launcher_deprecated.PodStatus`
* `airflow.kubernetes.pod_launcher_deprecated.get_kube_client` → `airflow.providers.cncf.kubernetes.kube_client.get_kube_client`
* `airflow.kubernetes.pod_launcher_deprecated.PodDefaults` → `airflow.providers.cncf.kubernetes.pod_generator_deprecated.PodDefaults`
* `airflow.kubernetes.pod_runtime_info_env.PodRuntimeInfoEnv` → `kubernetes.client.models.V1EnvVar`
* `airflow.kubernetes.volume.Volume` → `kubernetes.client.models.V1Volume`
* `airflow.kubernetes.volume_mount.VolumeMount` → `kubernetes.client.models.V1VolumeMount`
* `airflow.kubernetes.k8s_model.K8SModel` → `airflow.providers.cncf.kubernetes.k8s_model.K8SModel`
* `airflow.kubernetes.k8s_model.append_to_pod` → `airflow.providers.cncf.kubernetes.k8s_model.append_to_pod`
* `airflow.kubernetes.kube_client._disable_verify_ssl` → `airflow.kubernetes.airflow.providers.cncf.kubernetes.kube_client._disable_verify_ssl`
* `airflow.kubernetes.kube_client._enable_tcp_keepalive` → `airflow.kubernetes.airflow.providers.cncf.kubernetes.kube_client._enable_tcp_keepalive`
* `airflow.kubernetes.kube_client.get_kube_client` → `airflow.kubernetes.airflow.providers.cncf.kubernetes.kube_client.get_kube_client`
* `airflow.kubernetes.pod_generator.datetime_to_label_safe_datestring` → `airflow.providers.cncf.kubernetes.pod_generator.datetime_to_label_safe_datestring`
* `airflow.kubernetes.pod_generator.extend_object_field` → `airflow.kubernetes.airflow.providers.cncf.kubernetes.pod_generator.extend_object_field`
* `airflow.kubernetes.pod_generator.label_safe_datestring_to_datetime` → `airflow.providers.cncf.kubernetes.pod_generator.label_safe_datestring_to_datetime`
* `airflow.kubernetes.pod_generator.make_safe_label_value` → `airflow.providers.cncf.kubernetes.pod_generator.make_safe_label_value`
* `airflow.kubernetes.pod_generator.merge_objects` → `airflow.providers.cncf.kubernetes.pod_generator.merge_objects`
* `airflow.kubernetes.pod_generator.PodGenerator` → `airflow.providers.cncf.kubernetes.pod_generator.PodGenerator`
* `airflow.kubernetes.pod_generator.PodGeneratorDeprecated` → `airflow.providers.cncf.kubernetes.pod_generator.PodGenerator`
* `airflow.kubernetes.pod_generator.PodDefaults` → `airflow.providers.cncf.kubernetes.pod_generator_deprecated.PodDefaults`
* `airflow.kubernetes.pod_generator.add_pod_suffix` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.add_pod_suffix`
* `airflow.kubernetes.pod_generator.rand_str` → `airflow.providers.cncf.kubernetes.kubernetes_helper_functions.rand_str`
* `airflow.kubernetes.pod_generator_deprecated.make_safe_label_value` → `airflow.providers.cncf.kubernetes.pod_generator_deprecated.make_safe_label_value`
* `airflow.kubernetes.pod_generator_deprecated.PodDefaults` → `airflow.providers.cncf.kubernetes.pod_generator_deprecated.PodDefaults`
* `airflow.kubernetes.pod_generator_deprecated.PodGenerator` → `airflow.providers.cncf.kubernetes.pod_generator_deprecated.PodGenerator`
* `airflow.kubernetes.secret.Secret` → `airflow.providers.cncf.kubernetes.secret.Secret`
* `airflow.kubernetes.secret.K8SModel` → `airflow.providers.cncf.kubernetes.k8s_model.K8SModel`



## Test Plan

<!-- How was it tested? -->

A test fixture has been included for the rule.

---

_Comment by @github-actions[bot] on 2025-01-02 03:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Lee-W on 2025-01-02 03:12_

I think this is the last PR that contains this many rules 

---

_@dhruvmanila approved on 2025-01-02 04:59_

Thank you! I think we should do a similar refactor as I did in https://github.com/astral-sh/ruff/commit/86bdc2e7b1b3b6b8630a898fdcfc1071d5f7cb7d to simplify the match expression. I can do that as a follow-up, shouldn't be more than a couple of minutes.

---

_Renamed from " [airflow]: extend names moved from core to provider (AIR303)" to "[`airflow`] Extend names moved from core to provider (`AIR303`)" by @dhruvmanila on 2025-01-02 04:59_

---

_Label `rule` added by @dhruvmanila on 2025-01-02 04:59_

---

_Label `preview` added by @dhruvmanila on 2025-01-02 04:59_

---

_Merged by @dhruvmanila on 2025-01-02 05:00_

---

_Closed by @dhruvmanila on 2025-01-02 05:00_

---

_Comment by @Lee-W on 2025-01-02 05:09_

Would love to try it! Will create a PR later today

---

_Comment by @Lee-W on 2025-01-02 12:43_

just created https://github.com/astral-sh/ruff/pull/15220

---
