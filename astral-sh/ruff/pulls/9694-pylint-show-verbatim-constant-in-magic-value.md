```yaml
number: 9694
title: "[`pylint`] Show verbatim constant in `magic-value-comparison` (`PLR2004`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
assignees: []
merged: true
base: main
head: improve-PLR2004
created_at: 2024-01-30T01:14:55Z
updated_at: 2024-01-30T05:22:09Z
url: https://github.com/astral-sh/ruff/pull/9694
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Show verbatim constant in `magic-value-comparison` (`PLR2004`)

---

_Pull request opened by @diceroll123 on 2024-01-30 01:14_

## Summary

Tweaks PLR2004 to show the literal source text, rather than the constant value.

I noticed this when I had a hexadecimal constant, and the linter turned it into base-10.

Now, if you have `0x300`, it will show `0x300` instead of `768`.

Also, added backticks around the constant in the output message.

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-30 01:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3007 -3007 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1897 -1897 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_connexion/security.py#L50'>airflow/api_connexion/security.py:50:36:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_connexion/security.py#L50'>airflow/api_connexion/security.py:50:36:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_internal/internal_api_call.py#L104'>airflow/api_internal/internal_api_call.py:104:36:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_internal/internal_api_call.py#L104'>airflow/api_internal/internal_api_call.py:104:36:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/cli/commands/kubernetes_command.py#L83'>airflow/cli/commands/kubernetes_command.py:83:30:</a> PLR2004 Magic value used in comparison, consider replacing 5 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/cli/commands/kubernetes_command.py#L83'>airflow/cli/commands/kubernetes_command.py:83:30:</a> PLR2004 Magic value used in comparison, consider replacing `5` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/configuration.py#L2196'>airflow/configuration.py:2196:37:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/configuration.py#L2196'>airflow/configuration.py:2196:37:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/example_dags/example_kubernetes_executor.py#L135'>airflow/example_dags/example_kubernetes_executor.py:135:28:</a> PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/example_dags/example_kubernetes_executor.py#L135'>airflow/example_dags/example_kubernetes_executor.py:135:28:</a> PLR2004 Magic value used in comparison, consider replacing `4` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/executors/base_executor.py#L449'>airflow/executors/base_executor.py:449:27:</a> PLR2004 Magic value used in comparison, consider replacing 3 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/executors/base_executor.py#L449'>airflow/executors/base_executor.py:449:27:</a> PLR2004 Magic value used in comparison, consider replacing `3` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L481'>airflow/jobs/triggerer_job_runner.py:481:49:</a> PLR2004 Magic value used in comparison, consider replacing 60 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L481'>airflow/jobs/triggerer_job_runner.py:481:49:</a> PLR2004 Magic value used in comparison, consider replacing `60` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L577'>airflow/jobs/triggerer_job_runner.py:577:31:</a> PLR2004 Magic value used in comparison, consider replacing 0.2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L577'>airflow/jobs/triggerer_job_runner.py:577:31:</a> PLR2004 Magic value used in comparison, consider replacing `0.2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L401'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:401:26:</a> PLR2004 Magic value used in comparison, consider replacing 253 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L401'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:401:26:</a> PLR2004 Magic value used in comparison, consider replacing `253` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L230'>airflow/models/connection.py:230:35:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L230'>airflow/models/connection.py:230:35:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L232'>airflow/models/connection.py:232:54:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L232'>airflow/models/connection.py:232:54:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/airbyte/hooks/airbyte.py#L189'>airflow/providers/airbyte/hooks/airbyte.py:189:35:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/airbyte/hooks/airbyte.py#L189'>airflow/providers/airbyte/hooks/airbyte.py:189:35:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L261'>airflow/providers/amazon/aws/hooks/base_aws.py:261:40:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L261'>airflow/providers/amazon/aws/hooks/base_aws.py:261:40:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L870'>airflow/providers/amazon/aws/hooks/base_aws.py:870:50:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L870'>airflow/providers/amazon/aws/hooks/base_aws.py:870:50:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/chime.py#L88'>airflow/providers/amazon/aws/hooks/chime.py:88:27:</a> PLR2004 Magic value used in comparison, consider replacing 4096 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/chime.py#L88'>airflow/providers/amazon/aws/hooks/chime.py:88:27:</a> PLR2004 Magic value used in comparison, consider replacing `4096` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/emr.py#L168'>airflow/providers/amazon/aws/hooks/emr.py:168:62:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
... 3763 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+13 -13 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/integration/testdata/remote_invoke/lambda-fns/main.py#L37'>tests/integration/testdata/remote_invoke/lambda-fns/main.py:37:26:</a> PLR2004 Magic value used in comparison, consider replacing 100 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/integration/testdata/remote_invoke/lambda-fns/main.py#L37'>tests/integration/testdata/remote_invoke/lambda-fns/main.py:37:26:</a> PLR2004 Magic value used in comparison, consider replacing `100` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L19'>tests/smoke/download_sar_templates.py:19:47:</a> PLR2004 Magic value used in comparison, consider replacing 10 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L19'>tests/smoke/download_sar_templates.py:19:47:</a> PLR2004 Magic value used in comparison, consider replacing `10` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L24'>tests/smoke/download_sar_templates.py:24:50:</a> PLR2004 Magic value used in comparison, consider replacing 10 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L24'>tests/smoke/download_sar_templates.py:24:50:</a> PLR2004 Magic value used in comparison, consider replacing `10` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py#L127'>tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py:127:39:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py#L127'>tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py:127:39:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/package/test_artifact_exporter.py#L1849'>tests/unit/lib/package/test_artifact_exporter.py:1849:70:</a> PLR2004 Magic value used in comparison, consider replacing 32841 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/package/test_artifact_exporter.py#L1849'>tests/unit/lib/package/test_artifact_exporter.py:1849:70:</a> PLR2004 Magic value used in comparison, consider replacing `0o100111` with a constant variable
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+930 -930 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/filter_boolean.py#L7'>examples/basic/data/filter_boolean.py:7:26:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/filter_boolean.py#L7'>examples/basic/data/filter_boolean.py:7:26:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:50:</a> PLR2004 Magic value used in comparison, consider replacing 250 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:50:</a> PLR2004 Magic value used in comparison, consider replacing `250` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:61:</a> PLR2004 Magic value used in comparison, consider replacing 100 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:61:</a> PLR2004 Magic value used in comparison, consider replacing `100` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/elements.py#L21'>examples/basic/scatters/elements.py:21:50:</a> PLR2004 Magic value used in comparison, consider replacing 82 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/elements.py#L21'>examples/basic/scatters/elements.py:21:50:</a> PLR2004 Magic value used in comparison, consider replacing `82` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/hover_tooltips_image.py#L7'>examples/interaction/tools/hover_tooltips_image.py:7:36:</a> PLR2004 Magic value used in comparison, consider replacing 0.5 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/hover_tooltips_image.py#L7'>examples/interaction/tools/hover_tooltips_image.py:7:36:</a> PLR2004 Magic value used in comparison, consider replacing `0.5` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L44'>examples/models/trail.py:44:46:</a> PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L44'>examples/models/trail.py:44:46:</a> PLR2004 Magic value used in comparison, consider replacing `4` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L45'>examples/models/trail.py:45:31:</a> PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L45'>examples/models/trail.py:45:31:</a> PLR2004 Magic value used in comparison, consider replacing `4` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L45'>examples/models/trail.py:45:46:</a> PLR2004 Magic value used in comparison, consider replacing 6 with a constant variable
... 1845 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+167 -167 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/counts.py#L466'>analytics/lib/counts.py:466:23:</a> PLR2004 Magic value used in comparison, consider replacing 60 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/counts.py#L466'>analytics/lib/counts.py:466:23:</a> PLR2004 Magic value used in comparison, consider replacing `60` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/fixtures.py#L58'>analytics/lib/fixtures.py:58:17:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/fixtures.py#L58'>analytics/lib/fixtures.py:58:17:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/tests/test_activity_views.py#L303'>analytics/tests/test_activity_views.py:303:20:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/tests/test_activity_views.py#L303'>analytics/tests/test_activity_views.py:303:20:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/installation_activity.py#L241'>analytics/views/installation_activity.py:241:48:</a> PLR2004 Magic value used in comparison, consider replacing 5 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/installation_activity.py#L241'>analytics/views/installation_activity.py:241:48:</a> PLR2004 Magic value used in comparison, consider replacing `5` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/support.py#L219'>analytics/views/support.py:219:25:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/support.py#L219'>analytics/views/support.py:219:25:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
... 324 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR2004 | 6014 | 3007 | 3007 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3007 -3007 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1897 -1897 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_connexion/security.py#L50'>airflow/api_connexion/security.py:50:36:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_connexion/security.py#L50'>airflow/api_connexion/security.py:50:36:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_internal/internal_api_call.py#L104'>airflow/api_internal/internal_api_call.py:104:36:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/api_internal/internal_api_call.py#L104'>airflow/api_internal/internal_api_call.py:104:36:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/cli/commands/kubernetes_command.py#L83'>airflow/cli/commands/kubernetes_command.py:83:30:</a> PLR2004 Magic value used in comparison, consider replacing 5 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/cli/commands/kubernetes_command.py#L83'>airflow/cli/commands/kubernetes_command.py:83:30:</a> PLR2004 Magic value used in comparison, consider replacing `5` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/configuration.py#L2196'>airflow/configuration.py:2196:37:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/configuration.py#L2196'>airflow/configuration.py:2196:37:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/example_dags/example_kubernetes_executor.py#L135'>airflow/example_dags/example_kubernetes_executor.py:135:28:</a> PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/example_dags/example_kubernetes_executor.py#L135'>airflow/example_dags/example_kubernetes_executor.py:135:28:</a> PLR2004 Magic value used in comparison, consider replacing `4` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/executors/base_executor.py#L449'>airflow/executors/base_executor.py:449:27:</a> PLR2004 Magic value used in comparison, consider replacing 3 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/executors/base_executor.py#L449'>airflow/executors/base_executor.py:449:27:</a> PLR2004 Magic value used in comparison, consider replacing `3` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L481'>airflow/jobs/triggerer_job_runner.py:481:49:</a> PLR2004 Magic value used in comparison, consider replacing 60 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L481'>airflow/jobs/triggerer_job_runner.py:481:49:</a> PLR2004 Magic value used in comparison, consider replacing `60` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L577'>airflow/jobs/triggerer_job_runner.py:577:31:</a> PLR2004 Magic value used in comparison, consider replacing 0.2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/jobs/triggerer_job_runner.py#L577'>airflow/jobs/triggerer_job_runner.py:577:31:</a> PLR2004 Magic value used in comparison, consider replacing `0.2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L401'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:401:26:</a> PLR2004 Magic value used in comparison, consider replacing 253 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py#L401'>airflow/kubernetes/pre_7_4_0_compatibility/pod_generator.py:401:26:</a> PLR2004 Magic value used in comparison, consider replacing `253` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L230'>airflow/models/connection.py:230:35:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L230'>airflow/models/connection.py:230:35:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L232'>airflow/models/connection.py:232:54:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/models/connection.py#L232'>airflow/models/connection.py:232:54:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/airbyte/hooks/airbyte.py#L189'>airflow/providers/airbyte/hooks/airbyte.py:189:35:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/airbyte/hooks/airbyte.py#L189'>airflow/providers/airbyte/hooks/airbyte.py:189:35:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L261'>airflow/providers/amazon/aws/hooks/base_aws.py:261:40:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L261'>airflow/providers/amazon/aws/hooks/base_aws.py:261:40:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L870'>airflow/providers/amazon/aws/hooks/base_aws.py:870:50:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/base_aws.py#L870'>airflow/providers/amazon/aws/hooks/base_aws.py:870:50:</a> PLR2004 Magic value used in comparison, consider replacing `200` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/chime.py#L88'>airflow/providers/amazon/aws/hooks/chime.py:88:27:</a> PLR2004 Magic value used in comparison, consider replacing 4096 with a constant variable
+ <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/chime.py#L88'>airflow/providers/amazon/aws/hooks/chime.py:88:27:</a> PLR2004 Magic value used in comparison, consider replacing `4096` with a constant variable
- <a href='https://github.com/apache/airflow/blob/8914e49551d8ae5ece7418950b011c1f338b4634/airflow/providers/amazon/aws/hooks/emr.py#L168'>airflow/providers/amazon/aws/hooks/emr.py:168:62:</a> PLR2004 Magic value used in comparison, consider replacing 200 with a constant variable
... 3763 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+13 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/integration/testdata/remote_invoke/lambda-fns/main.py#L37'>tests/integration/testdata/remote_invoke/lambda-fns/main.py:37:26:</a> PLR2004 Magic value used in comparison, consider replacing 100 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/integration/testdata/remote_invoke/lambda-fns/main.py#L37'>tests/integration/testdata/remote_invoke/lambda-fns/main.py:37:26:</a> PLR2004 Magic value used in comparison, consider replacing `100` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L19'>tests/smoke/download_sar_templates.py:19:47:</a> PLR2004 Magic value used in comparison, consider replacing 10 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L19'>tests/smoke/download_sar_templates.py:19:47:</a> PLR2004 Magic value used in comparison, consider replacing `10` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L24'>tests/smoke/download_sar_templates.py:24:50:</a> PLR2004 Magic value used in comparison, consider replacing 10 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/smoke/download_sar_templates.py#L24'>tests/smoke/download_sar_templates.py:24:50:</a> PLR2004 Magic value used in comparison, consider replacing `10` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py#L127'>tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py:127:39:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py#L127'>tests/unit/lib/bootstrap/companion_stack/test_companion_stack_manager.py:127:39:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/package/test_artifact_exporter.py#L1849'>tests/unit/lib/package/test_artifact_exporter.py:1849:70:</a> PLR2004 Magic value used in comparison, consider replacing 32841 with a constant variable
+ <a href='https://github.com/aws/aws-sam-cli/blob/163ea90cd96fbb2271cdc168fb9a74095b66a013/tests/unit/lib/package/test_artifact_exporter.py#L1849'>tests/unit/lib/package/test_artifact_exporter.py:1849:70:</a> PLR2004 Magic value used in comparison, consider replacing `0o100111` with a constant variable
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+930 -930 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/filter_boolean.py#L7'>examples/basic/data/filter_boolean.py:7:26:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/filter_boolean.py#L7'>examples/basic/data/filter_boolean.py:7:26:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:50:</a> PLR2004 Magic value used in comparison, consider replacing 250 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:50:</a> PLR2004 Magic value used in comparison, consider replacing `250` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:61:</a> PLR2004 Magic value used in comparison, consider replacing 100 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/linked_brushing_subsets.py#L13'>examples/basic/data/linked_brushing_subsets.py:13:61:</a> PLR2004 Magic value used in comparison, consider replacing `100` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/elements.py#L21'>examples/basic/scatters/elements.py:21:50:</a> PLR2004 Magic value used in comparison, consider replacing 82 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/elements.py#L21'>examples/basic/scatters/elements.py:21:50:</a> PLR2004 Magic value used in comparison, consider replacing `82` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/hover_tooltips_image.py#L7'>examples/interaction/tools/hover_tooltips_image.py:7:36:</a> PLR2004 Magic value used in comparison, consider replacing 0.5 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/hover_tooltips_image.py#L7'>examples/interaction/tools/hover_tooltips_image.py:7:36:</a> PLR2004 Magic value used in comparison, consider replacing `0.5` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L44'>examples/models/trail.py:44:46:</a> PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L44'>examples/models/trail.py:44:46:</a> PLR2004 Magic value used in comparison, consider replacing `4` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L45'>examples/models/trail.py:45:31:</a> PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L45'>examples/models/trail.py:45:31:</a> PLR2004 Magic value used in comparison, consider replacing `4` with a constant variable
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/trail.py#L45'>examples/models/trail.py:45:46:</a> PLR2004 Magic value used in comparison, consider replacing 6 with a constant variable
... 1845 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+167 -167 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/counts.py#L466'>analytics/lib/counts.py:466:23:</a> PLR2004 Magic value used in comparison, consider replacing 60 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/counts.py#L466'>analytics/lib/counts.py:466:23:</a> PLR2004 Magic value used in comparison, consider replacing `60` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/fixtures.py#L58'>analytics/lib/fixtures.py:58:17:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/lib/fixtures.py#L58'>analytics/lib/fixtures.py:58:17:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/tests/test_activity_views.py#L303'>analytics/tests/test_activity_views.py:303:20:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/tests/test_activity_views.py#L303'>analytics/tests/test_activity_views.py:303:20:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/installation_activity.py#L241'>analytics/views/installation_activity.py:241:48:</a> PLR2004 Magic value used in comparison, consider replacing 5 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/installation_activity.py#L241'>analytics/views/installation_activity.py:241:48:</a> PLR2004 Magic value used in comparison, consider replacing `5` with a constant variable
- <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/support.py#L219'>analytics/views/support.py:219:25:</a> PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
+ <a href='https://github.com/zulip/zulip/blob/1a9441ec70e0ad5b6132559c8ceda9e875539b48/analytics/views/support.py#L219'>analytics/views/support.py:219:25:</a> PLR2004 Magic value used in comparison, consider replacing `2` with a constant variable
... 324 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR2004 | 6014 | 3007 | 3007 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-01-30 05:21_

Thanks! Makes sense to me.

---

_Label `bug` added by @charliermarsh on 2024-01-30 05:21_

---

_Renamed from "[`pylint`] - improve `PLR2004` (`magic-value-comparison`) to show the constant itself instead of the value" to "[`pylint`] Improve `magic-value-comparison` (`PLR2004`) to show verbatim constant" by @charliermarsh on 2024-01-30 05:21_

---

_Renamed from "[`pylint`] Improve `magic-value-comparison` (`PLR2004`) to show verbatim constant" to "[`pylint`] Show verbatim constant in `magic-value-comparison` (`PLR2004`)" by @charliermarsh on 2024-01-30 05:22_

---

_Merged by @charliermarsh on 2024-01-30 05:22_

---

_Closed by @charliermarsh on 2024-01-30 05:22_

---
