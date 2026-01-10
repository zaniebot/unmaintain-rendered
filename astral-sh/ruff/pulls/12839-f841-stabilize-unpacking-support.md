```yaml
number: 12839
title: "F841: Stabilize unpacking support"
type: pull_request
state: closed
author: MichaReiser
labels:
  - rule
  - needs-decision
assignees: []
base: ruff-0.6
head: stabalize-f841-unpacking
created_at: 2024-08-12T10:27:55Z
updated_at: 2024-08-12T11:20:44Z
url: https://github.com/astral-sh/ruff/pull/12839
synced_at: 2026-01-10T21:38:32Z
```

# F841: Stabilize unpacking support

---

_Pull request opened by @MichaReiser on 2024-08-12 10:27_

## Summary

This PR stabilizes the behavior change of F841 to support unpacking. See https://github.com/astral-sh/ruff/pull/8489/

## Open question

I'm unsure if we want to stabilize this change (in which case we should remove the implementation) because unpacking with too many values is an error. 


```
>>> a = (1, 2)
>>> x, = a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 1)
>>> x, _ = a
>>> 
```

## Test Plan

I reviewed the snapshot changes


---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-12 10:28_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-12 10:28_

---

_Label `rule` added by @MichaReiser on 2024-08-12 10:28_

---

_Comment by @github-actions[bot] on 2024-08-12 10:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1180 -1 violations, +0 -0 fixes in 37 projects; 17 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/telemetry.py#L594'>rasa/telemetry.py:594:30:</a> F841 Local variable `tb` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/telemetry.py#L594'>rasa/telemetry.py:594:9:</a> F841 Local variable `exc_type` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/cli/test_rasa_export.py#L278'>tests/cli/test_rasa_export.py:278:5:</a> F841 Local variable `events` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/evaluation/test_marker.py#L287'>tests/core/evaluation/test_marker.py:287:13:</a> F841 Local variable `expected` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/evaluation/test_marker.py#L307'>tests/core/evaluation/test_marker.py:307:13:</a> F841 Local variable `expected` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/evaluation/test_marker.py#L558'>tests/core/evaluation/test_marker.py:558:13:</a> F841 Local variable `expected_size` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/evaluation/test_marker.py#L583'>tests/core/evaluation/test_marker.py:583:13:</a> F841 Local variable `expected_size` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_evaluation.py#L152'>tests/core/test_evaluation.py:152:23:</a> F841 Local variable `num_stories` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_evaluation.py#L319'>tests/core/test_evaluation.py:319:23:</a> F841 Local variable `num_stories` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/training/test_interactive.py#L818'>tests/core/training/test_interactive.py:818:11:</a> F841 Local variable `kwargs` is assigned to but never used
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/computational_backends/feature_set.py#L113'>featuretools/computational_backends/feature_set.py:113:9:</a> F841 Local variable `node_needs_full_dataframe` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/primitives/utils.py#L122'>featuretools/primitives/utils.py:122:9:</a> F841 Local variable `trans_valid_inputs` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/primitives/utils.py#L123'>featuretools/primitives/utils.py:123:9:</a> F841 Local variable `trans_return_type` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/primitives/utils.py#L129'>featuretools/primitives/utils.py:129:9:</a> F841 Local variable `agg_valid_inputs` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/primitives/utils.py#L130'>featuretools/primitives/utils.py:130:9:</a> F841 Local variable `agg_return_type` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/computational_backend/test_calculate_feature_matrix.py#L1562'>featuretools/tests/computational_backend/test_calculate_feature_matrix.py:1562:9:</a> F841 Local variable `client` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/computational_backend/test_calculate_feature_matrix.py#L1601'>featuretools/tests/computational_backend/test_calculate_feature_matrix.py:1601:9:</a> F841 Local variable `client` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/primitive_tests/test_agg_feats.py#L596'>featuretools/tests/primitive_tests/test_agg_feats.py:596:9:</a> F841 Local variable `features` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/primitive_tests/test_agg_feats.py#L668'>featuretools/tests/primitive_tests/test_agg_feats.py:668:9:</a> F841 Local variable `features` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/8c99ff7d62d723e90d406ac56bae22aad82e6a47/featuretools/tests/primitive_tests/test_agg_feats.py#L793'>featuretools/tests/primitive_tests/test_agg_feats.py:793:9:</a> F841 Local variable `features` is assigned to but never used
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+92 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/fit_functions.py#L898'>src/plasmapy/analysis/fit_functions.py:898:22:</a> F841 Local variable `b` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/nullpoint.py#L1384'>src/plasmapy/analysis/nullpoint.py:1384:17:</a> F841 Local variable `eigen_vectors` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/nullpoint.py#L1384'>src/plasmapy/analysis/nullpoint.py:1384:5:</a> F841 Local variable `eigen_vals` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/nullpoint.py#L461'>src/plasmapy/analysis/nullpoint.py:461:5:</a> F841 Local variable `ax` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/nullpoint.py#L462'>src/plasmapy/analysis/nullpoint.py:462:5:</a> F841 Local variable `ay` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/nullpoint.py#L463'>src/plasmapy/analysis/nullpoint.py:463:5:</a> F841 Local variable `az` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/time_series/conditional_averaging.py#L154'>src/plasmapy/analysis/time_series/conditional_averaging.py:154:25:</a> F841 Local variable `_` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L1230'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:1230:12:</a> F841 Local variable `y` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L1230'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:1230:9:</a> F841 Local variable `x` is assigned to but never used
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/langmuir.py#L580'>src/plasmapy/diagnostics/langmuir.py:580:5:</a> F841 Local variable `_` is assigned to but never used
... 82 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+82 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/cli/commands/celery_command.py#L201'>airflow/cli/commands/celery_command.py:201:27:</a> F841 Local variable `stdout` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/cli/commands/celery_command.py#L201'>airflow/cli/commands/celery_command.py:201:35:</a> F841 Local variable `stderr` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/cli/commands/celery_command.py#L201'>airflow/cli/commands/celery_command.py:201:43:</a> F841 Local variable `log_file` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/configuration.py#L1619'>airflow/configuration.py:1619:22:</a> F841 Local variable `source` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/configuration.py#L686'>airflow/configuration.py:686:25:</a> F841 Local variable `should_continue` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/providers/celery/cli/celery_command.py#L217'>airflow/providers/celery/cli/celery_command.py:217:27:</a> F841 Local variable `stdout` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/providers/celery/cli/celery_command.py#L217'>airflow/providers/celery/cli/celery_command.py:217:35:</a> F841 Local variable `stderr` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/providers/celery/cli/celery_command.py#L217'>airflow/providers/celery/cli/celery_command.py:217:43:</a> F841 Local variable `log_file` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L399'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:399:26:</a> F841 Local variable `command` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/d12eb439603f896f22e4cd6f4e5daef22ae86254/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L399'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:399:35:</a> F841 Local variable `kube_executor_config` is assigned to but never used
... 72 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+11 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/superset/commands/report/alert.py#L155'>superset/commands/report/alert.py:155:13:</a> F841 Local variable `executor` is assigned to but never used
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/core_tests.py#L307'>tests/integration_tests/core_tests.py:307:21:</a> F841 Local variable `tests` is assigned to but never used
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/core_tests.py#L770'>tests/integration_tests/core_tests.py:770:27:</a> ARG002 Unused method argument: `key`
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/core_tests.py#L770'>tests/integration_tests/core_tests.py:770:27:</a> ARG002 Unused method argument: `key`
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/dict_import_export_tests.py#L166'>tests/integration_tests/dict_import_export_tests.py:166:9:</a> F841 Local variable `table` is assigned to but never used
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/dict_import_export_tests.py#L171'>tests/integration_tests/dict_import_export_tests.py:171:9:</a> F841 Local variable `table_over` is assigned to but never used
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/dict_import_export_tests.py#L195'>tests/integration_tests/dict_import_export_tests.py:195:9:</a> F841 Local variable `table` is assigned to but never used
+ <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/dict_import_export_tests.py#L200'>tests/integration_tests/dict_import_export_tests.py:200:9:</a> F841 Local variable `table_over` is assigned to but never used
... 5 additional changes omitted for rule F841
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+46 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L1005'>tests/integration/init/test_init_command.py:1005:13:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L1045'>tests/integration/init/test_init_command.py:1045:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L1080'>tests/integration/init/test_init_command.py:1080:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L118'>tests/integration/init/test_init_command.py:118:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L150'>tests/integration/init/test_init_command.py:150:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L182'>tests/integration/init/test_init_command.py:182:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L214'>tests/integration/init/test_init_command.py:214:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L248'>tests/integration/init/test_init_command.py:248:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L282'>tests/integration/init/test_init_command.py:282:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/51ca40470f6bd616c3a780e3376c6e0ef5900a83/tests/integration/init/test_init_command.py#L316'>tests/integration/init/test_init_command.py:316:17:</a> F841 Local variable `stdout_data` is assigned to but never used
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+44 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/Conversation_To_File.py#L78'>crazy_functions/Conversation_To_File.py:78:9:</a> F841 Local variable `html` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/agent_fns/python_comment_agent.py#L151'>crazy_functions/agent_fns/python_comment_agent.py:151:9:</a> F841 Local variable `begin` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/crazy_utils.py#L105'>crazy_functions/crazy_utils.py:105:21:</a> F841 Local variable `p_ratio` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/crazy_utils.py#L251'>crazy_functions/crazy_utils.py:251:21:</a> F841 Local variable `p_ratio` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/crazy_utils.py#L588'>crazy_functions/crazy_utils.py:588:13:</a> F841 Local variable `stdout` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/crazy_utils.py#L588'>crazy_functions/crazy_utils.py:588:21:</a> F841 Local variable `stderr` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/game_fns/game_interactive_story.py#L97'>crazy_functions/game_fns/game_interactive_story.py:97:13:</a> F841 Local variable `image_url` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/latex_fns/latex_toolbox.py#L603'>crazy_functions/latex_fns/latex_toolbox.py:603:17:</a> F841 Local variable `stderr` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/latex_fns/latex_toolbox.py#L603'>crazy_functions/latex_fns/latex_toolbox.py:603:9:</a> F841 Local variable `stdout` is assigned to but never used
+ <a href='https://github.com/binary-husky/gpt_academic/blob/6fe5f6ee6e42b9f5df212bab682d38bcf479c225/crazy_functions/pdf_fns/parse_pdf_via_doc2x.py#L145'>crazy_functions/pdf_fns/parse_pdf_via_doc2x.py:145:33:</a> F841 Local variable `project_folder` is assigned to but never used
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+25 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_model.py#L121'>src/bokeh/sphinxext/bokeh_model.py:121:34:</a> F841 Local variable `arglist` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_model.py#L121'>src/bokeh/sphinxext/bokeh_model.py:121:43:</a> F841 Local variable `retann` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_model.py#L121'>src/bokeh/sphinxext/bokeh_model.py:121:9:</a> F841 Local variable `name_prefix` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_options.py#L108'>src/bokeh/sphinxext/bokeh_options.py:108:36:</a> F841 Local variable `arglist` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_options.py#L108'>src/bokeh/sphinxext/bokeh_options.py:108:45:</a> F841 Local variable `retann` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_options.py#L108'>src/bokeh/sphinxext/bokeh_options.py:108:9:</a> F841 Local variable `name_prefix` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_plot.py#L254'>src/bokeh/sphinxext/bokeh_plot.py:254:15:</a> F841 Local variable `lineno` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_settings.py#L84'>src/bokeh/sphinxext/bokeh_settings.py:84:32:</a> F841 Local variable `arglist` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_settings.py#L84'>src/bokeh/sphinxext/bokeh_settings.py:84:41:</a> F841 Local variable `retann` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_settings.py#L84'>src/bokeh/sphinxext/bokeh_settings.py:84:9:</a> F841 Local variable `name_prefix` is assigned to but never used
... 15 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F841 | 1179 | 1179 | 0 | 0 | 0 |
| ARG002 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `needs-decision` added by @MichaReiser on 2024-08-12 10:49_

---

_Renamed from "F841: Stabalize unpacking support" to "F841: Stabilize unpacking support" by @AlexWaygood on 2024-08-12 10:54_

---

_Comment by @AlexWaygood on 2024-08-12 11:07_

> unpacking with too many values is an error

I'm not sure I understand how that's relevant to the behaviour being stabilised here. But I'm not sure we should stabilise this change while discussion in #8884 is still unresolved

---

_Comment by @MichaReiser on 2024-08-12 11:20_

Thanks. Yeah, what's mentioned in the issue is what I was concerned about. I'm starting to get to the conclusion that these should be separate rules. 

---

_Closed by @MichaReiser on 2024-08-12 11:20_

---
