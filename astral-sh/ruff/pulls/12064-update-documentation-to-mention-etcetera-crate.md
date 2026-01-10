```yaml
number: 12064
title: "Update documentation to mention `etcetera` crate instead of `dirs` for user configuration discovery"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: ruff-0.5
head: update-config-dir-discovery
created_at: 2024-06-27T07:58:26Z
updated_at: 2024-06-27T08:50:11Z
url: https://github.com/astral-sh/ruff/pull/12064
synced_at: 2026-01-10T21:56:00Z
```

# Update documentation to mention `etcetera` crate instead of `dirs` for user configuration discovery

---

_Pull request opened by @MichaReiser on 2024-06-27 07:58_

## Summary

We now use the `etcetera` crate to discover the configuration directory. This PR updates the documentation to reflect this change.




---

_Label `documentation` added by @MichaReiser on 2024-06-27 07:58_

---

_Comment by @github-actions[bot] on 2024-06-27 08:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+787 -36563 violations, +0 -0 fixes in 27 projects; 23 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/42634f8262a6fdb8eacdef16a064b5839bba7485/disnake/ext/commands/params.py#L807'>disnake/ext/commands/params.py:807:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/DisnakeDev/disnake/blob/42634f8262a6fdb8eacdef16a064b5839bba7485/tests/ext/commands/test_params.py#L143'>tests/ext/commands/test_params.py:143:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/DisnakeDev/disnake/blob/42634f8262a6fdb8eacdef16a064b5839bba7485/tests/ext/commands/test_params.py#L146'>tests/ext/commands/test_params.py:146:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/DisnakeDev/disnake/blob/42634f8262a6fdb8eacdef16a064b5839bba7485/tests/ext/commands/test_params.py#L214'>tests/ext/commands/test_params.py:214:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_validate_gpus.py#L26'>.github/tests/test_validate_gpus.py:26:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/utils/io.py#L66'>rasa/shared/utils/io.py:66:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/utils/io.py#L68'>rasa/shared/utils/io.py:68:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/utils/io.py#L93'>rasa/shared/utils/io.py:93:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/cli/test_utils.py#L458'>tests/cli/test_utils.py:458:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/cli/test_utils.py#L513'>tests/cli/test_utils.py:513:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/conftest.py#L891'>tests/conftest.py:891:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_actions.py#L2811'>tests/core/test_actions.py:2811:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/core/test_domain.py#L1856'>tests/shared/core/test_domain.py:1856:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/core/training_data/story_reader/test_yaml_story_reader.py#L272'>tests/shared/core/training_data/story_reader/test_yaml_story_reader.py:272:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/entityset_tests/test_es.py#L714'>featuretools/tests/entityset_tests/test_es.py:714:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/entityset_tests/test_serialization.py#L130'>featuretools/tests/entityset_tests/test_serialization.py:130:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/entityset_tests/test_serialization.py#L131'>featuretools/tests/entityset_tests/test_serialization.py:131:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/tests/particles/test_exceptions.py#L1040'>tests/particles/test_exceptions.py:1040:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+232 -25347 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/api/auth/backend/kerberos_auth.py#L69'>airflow/api/auth/backend/kerberos_auth.py:69:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/api/client/api_client.py#L28'>airflow/api/client/api_client.py:28:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/api/client/api_client.py#L34'>airflow/api/client/api_client.py:34:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/api/client/api_client.py#L46'>airflow/api/client/api_client.py:46:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/api/client/api_client.py#L53'>airflow/api/client/api_client.py:53:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/api/client/api_client.py#L60'>airflow/api/client/api_client.py:60:19:</a> ANN101 Missing type annotation for `self` in method
... 24623 additional changes omitted for rule ANN101
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/callbacks/callback_requests.py#L57'>airflow/callbacks/callback_requests.py:57:19:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/callbacks/callback_requests.py#L95'>airflow/callbacks/callback_requests.py:95:19:</a> ANN102 Missing type annotation for `cls` in classmethod
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:31:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:35:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:34:</a> S603 `subprocess` call: check for execution of untrusted input
... 297 additional changes omitted for rule S603
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/standalone_command.py#L51'>airflow/cli/commands/standalone_command.py:51:20:</a> ANN102 Missing type annotation for `cls` in classmethod
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/configuration.py#L1315'>airflow/configuration.py:1315:13:</a> SIM103 Return the condition `not value is None` directly
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/dag_processing/manager.py#L1268'>airflow/dag_processing/manager.py:1268:9:</a> SIM103 Return the condition `not self._num_run < self._max_runs` directly
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/dag_processing/manager.py#L498'>airflow/dag_processing/manager.py:498:9:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/dag_processing/processor.py#L422'>airflow/dag_processing/processor.py:422:21:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/dag_processing/processor.py#L803'>airflow/dag_processing/processor.py:803:21:</a> ANN102 Missing type annotation for `cls` in classmethod
... 539 additional changes omitted for rule ANN102
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:35:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:45:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:27:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:37:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/jobs/backfill_job_runner.py#L923'>airflow/jobs/backfill_job_runner.py:923:13:</a> FURB187 [*] Use of assignment of `reversed` on list `dagrun_infos`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/models/abstractoperator.py#L439'>airflow/models/abstractoperator.py:439:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/models/abstractoperator.py#L441'>airflow/models/abstractoperator.py:441:14:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/models/abstractoperator.py#L443'>airflow/models/abstractoperator.py:443:14:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/models/taskinstance.py#L671'>airflow/models/taskinstance.py:671:5:</a> SIM103 Return the condition `not isinstance(value, (bytearray, bytes, str))` directly
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/operators/python.py#L74'>airflow/operators/python.py:74:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/airbyte/triggers/airbyte.py#L120'>airflow/providers/airbyte/triggers/airbyte.py:120:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/amazon/aws/hooks/s3.py#L649'>airflow/providers/amazon/aws/hooks/s3.py:649:13:</a> SIM103 Return the negated condition directly
... 30 additional changes omitted for rule SIM103
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/apache/beam/hooks/beam.py#L575'>airflow/providers/apache/beam/hooks/beam.py:575:25:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/apache/beam/hooks/beam.py#L577'>airflow/providers/apache/beam/hooks/beam.py:577:13:</a> S604 Function call with `shell=True` parameter identified, security issue
... 25545 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+44 -4126 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L32'>examples/output/apis/autoload_static.py:32:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L34'>examples/output/apis/autoload_static.py:34:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L41'>examples/output/apis/autoload_static.py:41:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L43'>examples/output/apis/autoload_static.py:43:13:</a> ANN101 Missing type annotation for `self` in method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L45'>examples/output/apis/server_document/flask_server.py:45:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L46'>examples/output/apis/server_document/flask_server.py:46:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L15'>examples/server/api/tornado_embed.py:15:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L27'>examples/server/app/server_auth/auth.py:27:13:</a> ANN101 Missing type annotation for `self` in method
... 4003 additional changes omitted for rule ANN101
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> S602 `subprocess` call with `shell=True` identified, security issue
... 4160 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+166 -233 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/.github/github_workflow_scripts/utils_test.py#L228'>.github/github_workflow_scripts/utils_test.py:228:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/.github/github_workflow_scripts/utils_test.py#L303'>.github/github_workflow_scripts/utils_test.py:303:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/AHA/Integrations/AHA/AHA_test.py#L89'>Packs/AHA/Integrations/AHA/AHA_test.py:89:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
+ <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `self._valid(self.rcs)` directly
- <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L355'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:355:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L355'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:355:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/637a638fe906933c14eff0b870f6eecf4c6772f8/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L356'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:356:12:</a> E721 Do not compare types, use `isinstance()`
... 342 additional changes omitted for rule E721
... 389 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+10 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/admin/bootstrap.py#L120'>admin/bootstrap.py:120:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/admin/securedrop_admin/__init__.py#L598'>admin/securedrop_admin/__init__.py:598:12:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/admin/securedrop_admin/__init__.py#L600'>admin/securedrop_admin/__init__.py:600:12:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/admin/securedrop_admin/__init__.py#L604'>admin/securedrop_admin/__init__.py:604:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/devops/scripts/verify-mo.py#L116'>devops/scripts/verify-mo.py:116:16:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/devops/scripts/verify-mo.py#L120'>devops/scripts/verify-mo.py:120:26:</a> RUF100 [*] Unused `noqa` directive (unused: `S602`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `not pwd_flag == "NP"` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/models.py#L261'>securedrop/models.py:261:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `bool(self.fingerprint)` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
... 3 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (22 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN101 | 35378 | 0 | 35378 | 0 | 0 |
| ANN102 | 674 | 0 | 674 | 0 | 0 |
| E721 | 558 | 312 | 246 | 0 | 0 |
| S603 | 454 | 227 | 227 | 0 | 0 |
| SIM103 | 157 | 157 | 0 | 0 | 0 |
| S610 | 22 | 22 | 0 | 0 | 0 |
| S602 | 21 | 11 | 10 | 0 | 0 |
| S604 | 16 | 8 | 8 | 0 | 0 |
| FURB167 | 15 | 15 | 0 | 0 | 0 |
| FURB105 | 12 | 12 | 0 | 0 | 0 |
| PLR1701 | 11 | 0 | 11 | 0 | 0 |
| S605 | 8 | 4 | 4 | 0 | 0 |
| PERF403 | 6 | 6 | 0 | 0 | 0 |
| FURB129 | 6 | 6 | 0 | 0 | 0 |
| D107 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |
| E999 | 2 | 0 | 2 | 0 | 0 |
| E402 | 2 | 0 | 2 | 0 | 0 |
| FURB187 | 1 | 1 | 0 | 0 | 0 |
| FURB136 | 1 | 1 | 0 | 0 | 0 |
| FURB177 | 1 | 1 | 0 | 0 | 0 |
| RUF024 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+252 -252 violations, +0 -0 fixes in 7 projects; 1 project error; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+171 -174 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:31:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:35:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:34:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/cli/commands/internal_api_command.py#L179'>airflow/cli/commands/internal_api_command.py:179:22:</a> S603 `subprocess` call: check for execution of untrusted input
... 296 additional changes omitted for rule S603
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:35:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:45:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:27:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:37:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/apache/beam/hooks/beam.py#L575'>airflow/providers/apache/beam/hooks/beam.py:575:25:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/apache/beam/hooks/beam.py#L577'>airflow/providers/apache/beam/hooks/beam.py:577:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/providers/microsoft/azure/hooks/msgraph.py#L327'>airflow/providers/microsoft/azure/hooks/msgraph.py:327:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(data, (BytesIO, bytes, str))`
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/serialization/pydantic/dag.py#L45'>airflow/serialization/pydantic/dag.py:45:9:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/airflow/serialization/pydantic/taskinstance.py#L70'>airflow/serialization/pydantic/taskinstance.py:70:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, (BaseOperator, MappedOperator))`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1082'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1082:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1091'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1091:17:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1094'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1094:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1103'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1103:17:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1106'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1106:13:</a> S604 Function call with `shell=True` parameter identified, security issue
... 10 additional changes omitted for rule S604
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/hatch_build.py#L660'>hatch_build.py:660:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/hatch_build.py#L660'>hatch_build.py:660:59:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/hatch_build.py#L673'>hatch_build.py:673:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/hatch_build.py#L673'>hatch_build.py:673:59:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/scripts/ci/pre_commit/ruff_format.py#L26'>scripts/ci/pre_commit/ruff_format.py:26:1:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/scripts/ci/pre_commit/ruff_format.py#L26'>scripts/ci/pre_commit/ruff_format.py:26:33:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/tests/dags/test_on_kill.py#L44'>tests/dags/test_on_kill.py:44:13:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/tests/dags/test_on_kill.py#L44'>tests/dags/test_on_kill.py:44:23:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/tests/system/providers/amazon/aws/example_emr_eks.py#L102'>tests/system/providers/amazon/aws/example_emr_eks.py:102:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
... 10 additional changes omitted for rule S602
- <a href='https://github.com/apache/airflow/blob/bd64ac635c7d1680a639c65124d2bb9348f131fa/tests/task/task_runner/test_standard_task_runner.py#L340'>tests/task/task_runner/test_standard_task_runner.py:340:19:</a> S605 Starting a process with a shell, possible injection detected
... 2 additional changes omitted for rule S605
... 314 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+38 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L45'>examples/output/apis/server_document/flask_server.py:45:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L46'>examples/output/apis/server_document/flask_server.py:46:5:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:34:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:20:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:26:</a> S603 `subprocess` call: check for execution of untrusted input
... 69 additional changes omitted for rule S603
... 68 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/devops/scripts/verify-mo.py#L116'>devops/scripts/verify-mo.py:116:16:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/devops/scripts/verify-mo.py#L120'>devops/scripts/verify-mo.py:120:26:</a> RUF100 [*] Unused `noqa` directive (unused: `S602`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/fa604e6b37bc9c372f56d760537598a054b2c5e0/packaging/docker/entrypoint.py#L144'>packaging/docker/entrypoint.py:144:11:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/fa604e6b37bc9c372f56d760537598a054b2c5e0/packaging/docker/entrypoint.py#L144'>packaging/docker/entrypoint.py:144:28:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/fa604e6b37bc9c372f56d760537598a054b2c5e0/packaging/docker/entrypoint.py#L166'>packaging/docker/entrypoint.py:166:26:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/rotki/rotki/blob/fa604e6b37bc9c372f56d760537598a054b2c5e0/packaging/docker/entrypoint.py#L166'>packaging/docker/entrypoint.py:166:9:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/fa604e6b37bc9c372f56d760537598a054b2c5e0/packaging/docker/entrypoint.py#L174'>packaging/docker/entrypoint.py:174:52:</a> S602 `subprocess` call with `shell=True` seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/rotki/rotki/blob/fa604e6b37bc9c372f56d760537598a054b2c5e0/packaging/docker/entrypoint.py#L174'>packaging/docker/entrypoint.py:174:9:</a> S602 `subprocess` call with `shell=True` seems safe, but may be changed in the future; consider rewriting without `shell`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+37 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/check_rabbitmq_queue.py#L143'>scripts/lib/check_rabbitmq_queue.py:143:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/check_rabbitmq_queue.py#L144'>scripts/lib/check_rabbitmq_queue.py:144:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/check_rabbitmq_queue.py#L160'>scripts/lib/check_rabbitmq_queue.py:160:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/check_rabbitmq_queue.py#L161'>scripts/lib/check_rabbitmq_queue.py:161:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/hash_reqs.py#L38'>scripts/lib/hash_reqs.py:38:12:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/hash_reqs.py#L38'>scripts/lib/hash_reqs.py:38:36:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/puppet_cache.py#L25'>scripts/lib/puppet_cache.py:25:30:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/puppet_cache.py#L27'>scripts/lib/puppet_cache.py:27:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/setup_venv.py#L173'>scripts/lib/setup_venv.py:173:31:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/scripts/lib/setup_venv.py#L173'>scripts/lib/setup_venv.py:173:55:</a> S603 `subprocess` call: check for execution of untrusted input
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/agronholm/anyio">agronholm/anyio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/agronholm/anyio/blob/3ecc42273920c2828ba7b3303e860aaf59288cf3/tests/test_taskgroups.py#L557'>tests/test_taskgroups.py:557:27:</a> RUF100 [*] Unused `noqa` directive (unknown: `ASYNC101`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary>Changes by rule (6 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S603 | 454 | 227 | 227 | 0 | 0 |
| S602 | 21 | 11 | 10 | 0 | 0 |
| S604 | 16 | 8 | 8 | 0 | 0 |
| S605 | 8 | 4 | 4 | 0 | 0 |
| PLR1701 | 3 | 0 | 3 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---

_Review comment by @dhruvmanila on `docs/configuration.md`:266 on 2024-06-27 08:25_

I'm guessing this is a typo ;)

---

_@dhruvmanila approved on 2024-06-27 08:25_

---

_Merged by @MichaReiser on 2024-06-27 08:50_

---

_Closed by @MichaReiser on 2024-06-27 08:50_

---

_Branch deleted on 2024-06-27 08:50_

---
