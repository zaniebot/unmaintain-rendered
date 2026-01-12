```yaml
number: 8654
title: "Allow whitespace around colon in slices for `whitespace-before-punctuation` (`E203`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/E203
created_at: 2023-11-13T15:50:51Z
updated_at: 2023-11-27T07:18:12Z
url: https://github.com/astral-sh/ruff/pull/8654
synced_at: 2026-01-10T23:40:55Z
```

# Allow whitespace around colon in slices for `whitespace-before-punctuation` (`E203`)

---

_Pull request opened by @charliermarsh on 2023-11-13 15:50_

## Summary

This PR makes `whitespace-before-punctuation` (`E203`) compatible with the formatter by relaxing the rule a bit, as compared to the pycodestyle implementation. It's also more consistent with PEP 8, which says:

> However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority).

Closes https://github.com/astral-sh/ruff/issues/7259.
Closes https://github.com/astral-sh/ruff/issues/8642.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-11-13 15:50_

---

_Comment by @github-actions[bot] on 2023-11-13 16:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -132 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/59b101f923c193a2f58b7a844eae47a3fb7869c0/disnake/ext/commands/flag_converter.py#L516'>disnake/ext/commands/flag_converter.py:516:47:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/DisnakeDev/disnake/blob/59b101f923c193a2f58b7a844eae47a3fb7869c0/disnake/ext/commands/view.py#L102'>disnake/ext/commands/view.py:102:40:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/DisnakeDev/disnake/blob/59b101f923c193a2f58b7a844eae47a3fb7869c0/disnake/ext/commands/view.py#L63'>disnake/ext/commands/view.py:63:34:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/DisnakeDev/disnake/blob/59b101f923c193a2f58b7a844eae47a3fb7869c0/disnake/ext/commands/view.py#L76'>disnake/ext/commands/view.py:76:40:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/DisnakeDev/disnake/blob/59b101f923c193a2f58b7a844eae47a3fb7869c0/disnake/guild.py#L4404'>disnake/guild.py:4404:46:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/DisnakeDev/disnake/blob/59b101f923c193a2f58b7a844eae47a3fb7869c0/disnake/oggparse.py#L66'>disnake/oggparse.py:66:39:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/cli/utils.py#L385'>rasa/cli/utils.py:385:52:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/emulators/wit.py#L44'>rasa/nlu/emulators/wit.py:44:58:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/extractors/entity_synonyms.py#L72'>rasa/nlu/extractors/entity_synonyms.py:72:63:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/extractors/extractor.py#L284'>rasa/nlu/extractors/extractor.py:284:47:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/extractors/extractor.py#L335'>rasa/nlu/extractors/extractor.py:335:48:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/utils/bilou_utils.py#L278'>rasa/nlu/utils/bilou_utils.py:278:61:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/nlu/utils/bilou_utils.py#L280'>rasa/nlu/utils/bilou_utils.py:280:70:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/nlu/training_data/formats/readerwriter.py#L212'>rasa/shared/nlu/training_data/formats/readerwriter.py:212:50:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/nlu/training_data/formats/readerwriter.py#L228'>rasa/shared/nlu/training_data/formats/readerwriter.py:228:47:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/nlu/training_data/synonyms_parser.py#L22'>rasa/shared/nlu/training_data/synonyms_parser.py:22:54:</a> E203 [*] Whitespace before ':'
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/computational_backends/calculate_feature_matrix.py#L931'>featuretools/computational_backends/calculate_feature_matrix.py:931:55:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/computational_backends/calculate_feature_matrix.py#L935'>featuretools/computational_backends/calculate_feature_matrix.py:935:49:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L214'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:214:44:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/providers/google/cloud/operators/mlengine.py#L71'>airflow/providers/google/cloud/operators/mlengine.py:71:69:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/providers/google/cloud/operators/mlengine.py#L72'>airflow/providers/google/cloud/operators/mlengine.py:72:45:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/providers/google/cloud/transfers/gcs_to_gcs.py#L436'>airflow/providers/google/cloud/transfers/gcs_to_gcs.py:436:33:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/providers/google/cloud/transfers/s3_to_gcs.py#L318'>airflow/providers/google/cloud/transfers/s3_to_gcs.py:318:34:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/providers/microsoft/azure/hooks/wasb.py#L540'>airflow/providers/microsoft/azure/hooks/wasb.py:540:65:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/airflow/utils/helpers.py#L142'>airflow/utils/helpers.py:142:22:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/dev/breeze/src/airflow_breeze/utils/run_tests.py#L241'>dev/breeze/src/airflow_breeze/utils/run_tests.py:241:78:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/dev/breeze/src/airflow_breeze/utils/run_tests.py#L249'>dev/breeze/src/airflow_breeze/utils/run_tests.py:249:61:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/4cc98ba6648b4a350f9093c4e22ad6cff87ac5e4/dev/system_tests/update_issue_status.py#L157'>dev/system_tests/update_issue_status.py:157:35:</a> E203 [*] Whitespace before ':'
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/init/interactive_event_bridge_flow.py#L167'>samcli/commands/init/interactive_event_bridge_flow.py:167:32:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/hook_packages/terraform/hooks/prepare/makefile_generator.py#L255'>samcli/hook_packages/terraform/hooks/prepare/makefile_generator.py:255:34:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1032'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1032:60:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L481'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:481:77:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L873'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:873:64:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L937'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:937:64:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/commaai/openpilot/blob/9e06525642eb46892a58371cc8daa70fba95f02a/system/hardware/tici/esim.py#L76'>system/hardware/tici/esim.py:76:37:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/b001184a93d5067f2fc2e16848ec2555ec0c47e4/securedrop/models.py#L636'>securedrop/models.py:636:24:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/freedomofpress/securedrop/blob/b001184a93d5067f2fc2e16848ec2555ec0c47e4/securedrop/tests/test_journalist.py#L899'>securedrop/tests/test_journalist.py:899:28:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/freedomofpress/securedrop/blob/b001184a93d5067f2fc2e16848ec2555ec0c47e4/securedrop/tests/test_remove_pending_sources.py#L84'>securedrop/tests/test_remove_pending_sources.py:84:32:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/freedomofpress/securedrop/blob/b001184a93d5067f2fc2e16848ec2555ec0c47e4/securedrop/tests/test_remove_pending_sources.py#L88'>securedrop/tests/test_remove_pending_sources.py:88:36:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/dask/execution/generic.py#L343'>ibis/backends/dask/execution/generic.py:343:24:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/dask/execution/generic.py#L351'>ibis/backends/dask/execution/generic.py:351:24:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/dask/tests/execution/test_operations.py#L464'>ibis/backends/dask/tests/execution/test_operations.py:464:29:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/dask/tests/execution/test_operations.py#L478'>ibis/backends/dask/tests/execution/test_operations.py:478:43:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/dask/tests/execution/test_strings.py#L36'>ibis/backends/dask/tests/execution/test_strings.py:36:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/impala/__init__.py#L141'>ibis/backends/impala/__init__.py:141:23:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/impala/__init__.py#L154'>ibis/backends/impala/__init__.py:154:30:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/pandas/execution/generic.py#L101'>ibis/backends/pandas/execution/generic.py:101:28:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/pandas/execution/strings.py#L38'>ibis/backends/pandas/execution/strings.py:38:30:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/ad930b86767e9f4bfe8cbbde9f9f16c6c316bea2/ibis/backends/pandas/tests/execution/conftest.py#L82'>ibis/backends/pandas/tests/execution/conftest.py:82:20:</a> E203 [*] Whitespace before ':'
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/jrnl/journals/Journal.py#L191'>jrnl/journals/Journal.py:191:66:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/client/abstract.py#L363'>pymilvus/client/abstract.py:363:73:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/client/abstract.py#L368'>pymilvus/client/abstract.py:368:61:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/client/abstract.py#L419'>pymilvus/client/abstract.py:419:53:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/milvus_client/milvus_client.py#L176'>pymilvus/milvus_client/milvus_client.py:176:34:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/orm/iterator.py#L133'>pymilvus/orm/iterator.py:133:31:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/orm/iterator.py#L148'>pymilvus/orm/iterator.py:148:24:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/conftest.py#L226'>conftest.py:226:37:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/examples/paddle/train_low_level_api.py#L53'>examples/paddle/train_low_level_api.py:53:28:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/openai/__init__.py#L769'>mlflow/openai/__init__.py:769:41:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/openai/__init__.py#L799'>mlflow/openai/__init__.py:799:33:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/recipes/utils/execution.py#L96'>mlflow/recipes/utils/execution.py:96:31:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/store/artifact/gcs_artifact_repo.py#L125'>mlflow/store/artifact/gcs_artifact_repo.py:125:53:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/store/tracking/sqlalchemy_store.py#L745'>mlflow/store/tracking/sqlalchemy_store.py:745:34:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/store/tracking/sqlalchemy_store.py#L804'>mlflow/store/tracking/sqlalchemy_store.py:804:44:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/utils/__init__.py#L67'>mlflow/utils/__init__.py:67:18:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/6b88bb7b65e12b5cd2721e2a0a8feffa0787ae2f/mlflow/utils/autologging_utils/__init__.py#L239'>mlflow/utils/autologging_utils/__init__.py:239:24:</a> E203 [*] Whitespace before ':'
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/cli/autocompletion.py#L79'>src/pip/_internal/cli/autocompletion.py:79:55:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+0 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial002.py#L23'>docs_src/dependencies/tutorial002.py:23:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial002_an.py#L24'>docs_src/dependencies/tutorial002_an.py:24:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial002_an_py310.py#L23'>docs_src/dependencies/tutorial002_an_py310.py:23:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial002_an_py39.py#L23'>docs_src/dependencies/tutorial002_an_py39.py:23:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial002_py310.py#L21'>docs_src/dependencies/tutorial002_py310.py:21:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial003.py#L23'>docs_src/dependencies/tutorial003.py:23:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial003_an.py#L24'>docs_src/dependencies/tutorial003_an.py:24:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial003_an_py310.py#L23'>docs_src/dependencies/tutorial003_an_py310.py:23:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial003_an_py39.py#L23'>docs_src/dependencies/tutorial003_an_py39.py:23:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/tiangolo/fastapi/blob/480620372a662aa9025c47410fbc90a255b2fc94/docs_src/dependencies/tutorial003_py310.py#L21'>docs_src/dependencies/tutorial003_py310.py:21:39:</a> E203 [*] Whitespace before ':'
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/yandex/ch-backup/blob/070f87f52d8f5b3a0374da9b7b2630327501b08f/ch_backup/storage/stages/storage.py#L163'>ch_backup/storage/stages/storage.py:163:26:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/yandex/ch-backup/blob/070f87f52d8f5b3a0374da9b7b2630327501b08f/tests/unit/test_bytes_fifo.py#L133'>tests/unit/test_bytes_fifo.py:133:41:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/analytics/views/stats.py#L536'>analytics/views/stats.py:536:33:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L627'>tools/lib/template_parser.py:627:59:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L628'>tools/lib/template_parser.py:628:15:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L663'>tools/lib/template_parser.py:663:59:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L664'>tools/lib/template_parser.py:664:15:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L67'>tools/lib/template_parser.py:67:28:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L682'>tools/lib/template_parser.py:682:64:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L684'>tools/lib/template_parser.py:684:58:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L685'>tools/lib/template_parser.py:685:15:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/1b804cd0dd7480bb7067f577ab789d18c5f669bc/tools/lib/template_parser.py#L693'>tools/lib/template_parser.py:693:24:</a> E203 [*] Whitespace before ':'
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E203 | 132 | 0 | 132 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-13 17:16_

---

_Closed by @charliermarsh on 2023-11-13 17:16_

---

_Branch deleted on 2023-11-13 17:16_

---

_@ofek reviewed on 2023-11-13 17:21_

---

_Review comment by @ofek on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:106 on 2023-11-13 17:21_

Wouldn't this now be not okay, since the spacing is different on the other side?

---

_@charliermarsh reviewed on 2023-11-13 17:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:106 on 2023-11-13 17:28_

The intent was to allow _either_ this or the variant with the space.

---

_@ofek reviewed on 2023-11-13 17:29_

---

_Review comment by @ofek on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E20.py`:106 on 2023-11-13 17:29_

Oh okay, interesting choice

---

_@MichaReiser reviewed on 2023-11-27 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:141 on 2023-11-27 07:14_

I don't fully understand what has changed or how the new detection should work, which is why I'm unable to come up with a good example. 

Is it possible that this change does not work as intended for parenthesized expressions inside f-strings because they are in between the start end token (fstrings > 0)?

```python
f"start of f-string {a[(the_paren_logic) :b]} end of fstring"
```



---

_@MichaReiser reviewed on 2023-11-27 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:151 on 2023-11-27 07:16_

Nit

```suggestion
					TokenKind::Lsqb | TokenKind::Lbrace if fstrings == 0 => {
                brackets.push(kind);
            }
            TokenKind::Rsqb | TokenKind::Rbrace if fstrings == 0 => {
                brackets.pop();
            }
```


---

_@MichaReiser reviewed on 2023-11-27 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:203 on 2023-11-27 07:18_

Nit: 

```
			brackets.last() == Some(TokenKind::Lsqb)			
```



---
