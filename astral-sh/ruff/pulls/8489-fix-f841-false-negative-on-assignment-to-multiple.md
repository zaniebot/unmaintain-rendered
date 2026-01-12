```yaml
number: 8489
title: "Fix `F841` false negative on assignment to multiple variables"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: F841-fix
created_at: 2023-11-04T16:21:22Z
updated_at: 2023-11-05T17:20:43Z
url: https://github.com/astral-sh/ruff/pull/8489
synced_at: 2026-01-12T15:55:26Z
```

# Fix `F841` false negative on assignment to multiple variables

---

_@tjkuson_

## Summary

Closes #8441 behind preview feature flag.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-11-04 16:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/1590461a406f41dde4e0ec8d6db0ba097b3d45e2/rotkehlchen/tests/unit/decoders/test_makerdao_sai.py#L2250'>rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2250:13:</a> F841 Local variable `__annotations__` is assigned to but never used
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F841 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1417 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/telemetry.py#L594'>rasa/telemetry.py:594:30:</a> F841 Local variable `tb` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/telemetry.py#L594'>rasa/telemetry.py:594:9:</a> F841 Local variable `exc_type` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/cli/test_rasa_export.py#L278'>tests/cli/test_rasa_export.py:278:5:</a> F841 Local variable `events` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/evaluation/test_marker.py#L287'>tests/core/evaluation/test_marker.py:287:13:</a> F841 Local variable `expected` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/evaluation/test_marker.py#L307'>tests/core/evaluation/test_marker.py:307:13:</a> F841 Local variable `expected` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/evaluation/test_marker.py#L558'>tests/core/evaluation/test_marker.py:558:13:</a> F841 Local variable `expected_size` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/evaluation/test_marker.py#L583'>tests/core/evaluation/test_marker.py:583:13:</a> F841 Local variable `expected_size` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/test_evaluation.py#L152'>tests/core/test_evaluation.py:152:23:</a> F841 Local variable `num_stories` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/test_evaluation.py#L319'>tests/core/test_evaluation.py:319:23:</a> F841 Local variable `num_stories` is assigned to but never used
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/core/training/test_interactive.py#L818'>tests/core/training/test_interactive.py:818:11:</a> F841 Local variable `kwargs` is assigned to but never used
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/computational_backends/feature_set.py#L113'>featuretools/computational_backends/feature_set.py:113:9:</a> F841 Local variable `node_needs_full_dataframe` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/primitives/utils.py#L144'>featuretools/primitives/utils.py:144:9:</a> F841 Local variable `trans_valid_inputs` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/primitives/utils.py#L145'>featuretools/primitives/utils.py:145:9:</a> F841 Local variable `trans_return_type` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/primitives/utils.py#L151'>featuretools/primitives/utils.py:151:9:</a> F841 Local variable `agg_valid_inputs` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/primitives/utils.py#L152'>featuretools/primitives/utils.py:152:9:</a> F841 Local variable `agg_return_type` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/computational_backend/test_calculate_feature_matrix.py#L1668'>featuretools/tests/computational_backend/test_calculate_feature_matrix.py:1668:9:</a> F841 Local variable `client` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/computational_backend/test_calculate_feature_matrix.py#L1707'>featuretools/tests/computational_backend/test_calculate_feature_matrix.py:1707:9:</a> F841 Local variable `client` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/primitive_tests/test_agg_feats.py#L609'>featuretools/tests/primitive_tests/test_agg_feats.py:609:9:</a> F841 Local variable `features` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/primitive_tests/test_agg_feats.py#L683'>featuretools/tests/primitive_tests/test_agg_feats.py:683:9:</a> F841 Local variable `features` is assigned to but never used
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/primitive_tests/test_agg_feats.py#L809'>featuretools/tests/primitive_tests/test_agg_feats.py:809:9:</a> F841 Local variable `features` is assigned to but never used
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+65 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/cli/commands/celery_command.py#L192'>airflow/cli/commands/celery_command.py:192:16:</a> F841 Local variable `stderr` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/cli/commands/celery_command.py#L192'>airflow/cli/commands/celery_command.py:192:24:</a> F841 Local variable `log_file` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/cli/commands/celery_command.py#L192'>airflow/cli/commands/celery_command.py:192:8:</a> F841 Local variable `stdout` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/configuration.py#L1596'>airflow/configuration.py:1596:22:</a> F841 Local variable `source` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/configuration.py#L687'>airflow/configuration.py:687:25:</a> F841 Local variable `should_continue` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/io/store/path.py#L164'>airflow/io/store/path.py:164:31:</a> F841 Local variable `o_key` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/providers/google/cloud/log/gcs_task_handler.py#L178'>airflow/providers/google/cloud/log/gcs_task_handler.py:178:33:</a> F841 Local variable `stackinfo` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/providers/google/cloud/transfers/s3_to_gcs.py#L228'>airflow/providers/google/cloud/transfers/s3_to_gcs.py:228:9:</a> F841 Local variable `gcs_bucket` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/providers/google/cloud/transfers/s3_to_gcs.py#L242'>airflow/providers/google/cloud/transfers/s3_to_gcs.py:242:9:</a> F841 Local variable `gcs_bucket` is assigned to but never used
+ <a href='https://github.com/apache/airflow/blob/be2c3b9d9ee1140805716efd69eeba066e35bd23/airflow/providers/google/cloud/transfers/s3_to_gcs.py#L250'>airflow/providers/google/cloud/transfers/s3_to_gcs.py:250:30:</a> F841 Local variable `dest_gcs_object_prefix` is assigned to but never used
... 55 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+46 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L1012'>tests/integration/init/test_init_command.py:1012:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L118'>tests/integration/init/test_init_command.py:118:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L150'>tests/integration/init/test_init_command.py:150:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L182'>tests/integration/init/test_init_command.py:182:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L214'>tests/integration/init/test_init_command.py:214:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L248'>tests/integration/init/test_init_command.py:248:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L282'>tests/integration/init/test_init_command.py:282:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L316'>tests/integration/init/test_init_command.py:316:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L529'>tests/integration/init/test_init_command.py:529:17:</a> F841 Local variable `stdout_data` is assigned to but never used
+ <a href='https://github.com/aws/aws-sam-cli/blob/35eb3d763830dc01321fb4fcbdb152c0b68cc907/tests/integration/init/test_init_command.py#L558'>tests/integration/init/test_init_command.py:558:17:</a> F841 Local variable `stdout_data` is assigned to but never used
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+25 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_model.py#L117'>src/bokeh/sphinxext/bokeh_model.py:117:34:</a> F841 Local variable `arglist` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_model.py#L117'>src/bokeh/sphinxext/bokeh_model.py:117:43:</a> F841 Local variable `retann` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_model.py#L117'>src/bokeh/sphinxext/bokeh_model.py:117:9:</a> F841 Local variable `name_prefix` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_options.py#L104'>src/bokeh/sphinxext/bokeh_options.py:104:36:</a> F841 Local variable `arglist` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_options.py#L104'>src/bokeh/sphinxext/bokeh_options.py:104:45:</a> F841 Local variable `retann` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_options.py#L104'>src/bokeh/sphinxext/bokeh_options.py:104:9:</a> F841 Local variable `name_prefix` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_plot.py#L254'>src/bokeh/sphinxext/bokeh_plot.py:254:15:</a> F841 Local variable `lineno` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_settings.py#L84'>src/bokeh/sphinxext/bokeh_settings.py:84:32:</a> F841 Local variable `arglist` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_settings.py#L84'>src/bokeh/sphinxext/bokeh_settings.py:84:41:</a> F841 Local variable `retann` is assigned to but never used
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/src/bokeh/sphinxext/bokeh_settings.py#L84'>src/bokeh/sphinxext/bokeh_settings.py:84:9:</a> F841 Local variable `name_prefix` is assigned to but never used
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/0fbc36bce19a7fb7a57484488bed16f07a51c888/tools/sim/bridge/metadrive/metadrive_process.py#L90'>tools/sim/bridge/metadrive/metadrive_process.py:90:30:</a> F841 Local variable `info` is assigned to but never used
+ <a href='https://github.com/commaai/openpilot/blob/0fbc36bce19a7fb7a57484488bed16f07a51c888/tools/sim/bridge/metadrive/metadrive_process.py#L90'>tools/sim/bridge/metadrive/metadrive_process.py:90:7:</a> F841 Local variable `obs` is assigned to but never used
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+523 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Documentation/common_server_docs.py#L252'>Documentation/common_server_docs.py:252:5:</a> F841 Local variable `js_doc` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Documentation/common_server_docs.py#L256'>Documentation/common_server_docs.py:256:5:</a> F841 Local variable `ps_doc` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L762'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:762:14:</a> F841 Local variable `RET` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L762'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:762:29:</a> F841 Local variable `rmsg` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L762'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:762:9:</a> F841 Local variable `SDL` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L824'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:824:14:</a> F841 Local variable `RET` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L824'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:824:24:</a> F841 Local variable `rcs` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L824'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:824:29:</a> F841 Local variable `rmsg` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L839'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:839:14:</a> F841 Local variable `RET` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L839'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:839:24:</a> F841 Local variable `rcs` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L839'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:839:29:</a> F841 Local variable `rmsg` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L839'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:839:9:</a> F841 Local variable `SDL` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L856'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:856:24:</a> F841 Local variable `rcs` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L856'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:856:29:</a> F841 Local variable `rmsg` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L856'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:856:9:</a> F841 Local variable `SDL` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L241'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:241:13:</a> F841 Local variable `new_last_ids` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L241'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:241:27:</a> F841 Local variable `new_collect_from` is assigned to but never used
+ <a href='https://github.com/demisto/content/blob/0c711e312939e1d9ff5020ddf01366f3d71d97c1/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L303'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:303:13:</a> F841 Local variable `new_last_ids` is assigned to but never used
... 505 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/image.py#L401'>docker/api/image.py:401:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/image.py#L476'>docker/api/image.py:476:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/plugin.py#L132'>docker/api/plugin.py:132:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/plugin.py#L173'>docker/api/plugin.py:173:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/plugin.py#L199'>docker/api/plugin.py:199:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/plugin.py#L252'>docker/api/plugin.py:252:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/service.py#L166'>docker/api/service.py:166:19:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/api/service.py#L447'>docker/api/service.py:447:23:</a> F841 Local variable `repo_name` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/transport/npipesocket.py#L121'>docker/transport/npipesocket.py:121:9:</a> F841 Local variable `err` is assigned to but never used
+ <a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/docker/transport/npipesocket.py#L143'>docker/transport/npipesocket.py:143:13:</a> F841 Local variable `err` is assigned to but never used
... 21 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F841 | 1417 | 1417 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @tjkuson on 2023-11-04 17:15_

None of the new violations seem to be incorrect, but there are a lot of them. Moved the behaviour change behind the preview flag.

---

_Marked ready for review by @tjkuson on 2023-11-04 17:17_

---

_Converted to draft by @tjkuson on 2023-11-04 17:28_

---

_Marked ready for review by @tjkuson on 2023-11-04 17:36_

---

_Comment by @tjkuson on 2023-11-04 17:55_

I don't know why there is a non-preview ecosystem change...

Edit: it's because the repo enables them.

---

_Comment by @T-256 on 2023-11-04 18:21_

> I don't know why there is a non-preview ecosystem change...

rotki uses `preview = true` in `pyproject.toml`, I think there is problem in `ruff_ecosystem` pacakge.

possible solution is adding `--force-no-preview` to cli.

---

_Merged by @charliermarsh on 2023-11-05 17:01_

---

_Closed by @charliermarsh on 2023-11-05 17:01_

---

_Comment by @charliermarsh on 2023-11-05 17:01_

Thanks! Also confirmed the above (that Rotki enables preview on their `develop` branch).

---

_Branch deleted on 2023-11-05 17:20_

---
