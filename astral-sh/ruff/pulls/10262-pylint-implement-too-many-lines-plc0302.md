```yaml
number: 10262
title: "[`pylint`] Implement `too-many-lines` (`PLC0302`)"
type: pull_request
state: closed
author: augustelalande
labels: []
assignees: []
base: main
head: too-many-lines
created_at: 2024-03-07T01:45:46Z
updated_at: 2024-03-31T15:59:19Z
url: https://github.com/astral-sh/ruff/pull/10262
synced_at: 2026-01-12T15:55:31Z
```

# [`pylint`] Implement `too-many-lines` (`PLC0302`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements the [too-many-lines](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/too-many-lines.html) rule (C0302) from pylint.

## Test Plan

New fixtures have been added

Part of #970


---

_Renamed from "[`pylint`] implement `too-many-lines` (`PLC0302`)" to "[`pylint`] Implement `too-many-lines` (`PLC0302`)" by @augustelalande on 2024-03-07 01:45_

---

_Comment by @github-actions[bot] on 2024-03-07 02:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+143 -2 violations, +0 -0 fixes in 9 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/abc.py#L1'>disnake/abc.py:1:1:</a> PLC0302 Too many lines in module (2013>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/channel.py#L1'>disnake/channel.py:1:1:</a> PLC0302 Too many lines in module (5053>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/client.py#L1'>disnake/client.py:1:1:</a> PLC0302 Too many lines in module (3213>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/ext/commands/core.py#L1'>disnake/ext/commands/core.py:1:1:</a> PLC0302 Too many lines in module (2683>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/flags.py#L1'>disnake/flags.py:1:1:</a> PLC0302 Too many lines in module (2567>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/guild.py#L1'>disnake/guild.py:1:1:</a> PLC0302 Too many lines in module (5341>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/http.py#L1'>disnake/http.py:1:1:</a> PLC0302 Too many lines in module (2775>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/message.py#L1'>disnake/message.py:1:1:</a> PLC0302 Too many lines in module (2513>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/state.py#L1'>disnake/state.py:1:1:</a> PLC0302 Too many lines in module (2324>2000)
+ <a href='https://github.com/DisnakeDev/disnake/blob/d92ba6a606e9b51a08dfe08234b4c4fccd078f41/disnake/webhook/async_.py#L1'>disnake/webhook/async_.py:1:1:</a> PLC0302 Too many lines in module (2039>2000)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/b27109578006830d4aa89f182e2f9861c781b3a7/aiven/client/cli.py#L1'>aiven/client/cli.py:1:1:</a> PLC0302 Too many lines in module (6024>2000)
+ <a href='https://github.com/aiven/aiven-client/blob/b27109578006830d4aa89f182e2f9861c781b3a7/aiven/client/client.py#L1'>aiven/client/client.py:1:1:</a> PLC0302 Too many lines in module (2763>2000)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+30 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/cli/cli_config.py#L1'>airflow/cli/cli_config.py:1:1:</a> PLC0302 Too many lines in module (2128>2000)
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/configuration.py#L1'>airflow/configuration.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/configuration.py#L1'>airflow/configuration.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/configuration.py#L1'>airflow/configuration.py:1:1:</a> PLC0302 Too many lines in module (2348>2000)
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/models/dag.py#L1'>airflow/models/dag.py:1:1:</a> PLC0302 Too many lines in module (4199>2000)
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/models/taskinstance.py#L1'>airflow/models/taskinstance.py:1:1:</a> PLC0302 Too many lines in module (3772>2000)
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/providers/fab/auth_manager/security_manager/override.py#L1'>airflow/providers/fab/auth_manager/security_manager/override.py:1:1:</a> PLC0302 Too many lines in module (2765>2000)
+ <a href='https://github.com/apache/airflow/blob/095c5fe3137e2cb6d45e8f3184bae149cb2850d1/airflow/providers/google/cloud/hooks/bigquery.py#L1'>airflow/providers/google/cloud/hooks/bigquery.py:1:1:</a> PLC0302 Too many lines in module (3539>2000)
... 24 additional changes omitted for rule PLC0302
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1:1:</a> PLC0302 Too many lines in module (2447>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/integration/buildcmd/test_build_cmd.py#L1'>tests/integration/buildcmd/test_build_cmd.py:1:1:</a> PLC0302 Too many lines in module (3146>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/integration/local/start_api/test_start_api.py#L1'>tests/integration/local/start_api/test_start_api.py:1:1:</a> PLC0302 Too many lines in module (3253>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/commands/init/test_cli.py#L1'>tests/unit/commands/init/test_cli.py:1:1:</a> PLC0302 Too many lines in module (3195>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/commands/local/lib/test_sam_api_provider.py#L1'>tests/unit/commands/local/lib/test_sam_api_provider.py:1:1:</a> PLC0302 Too many lines in module (2273>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/commands/local/lib/test_sam_function_provider.py#L1'>tests/unit/commands/local/lib/test_sam_function_provider.py:1:1:</a> PLC0302 Too many lines in module (2560>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L1'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:1:1:</a> PLC0302 Too many lines in module (3132>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/lib/build_module/test_app_builder.py#L1'>tests/unit/lib/build_module/test_app_builder.py:1:1:</a> PLC0302 Too many lines in module (3015>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/lib/package/test_artifact_exporter.py#L1'>tests/unit/lib/package/test_artifact_exporter.py:1:1:</a> PLC0302 Too many lines in module (2024>2000)
+ <a href='https://github.com/aws/aws-sam-cli/blob/0307f10811541c4aea55a0d5c0c7593ae7545f19/tests/unit/local/apigw/test_local_apigw_service.py#L1'>tests/unit/local/apigw/test_local_apigw_service.py:1:1:</a> PLC0302 Too many lines in module (2039>2000)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/25eda43904cdfb481759a7c497716b4839956014/securedrop/tests/test_journalist.py#L1'>securedrop/tests/test_journalist.py:1:1:</a> PLC0302 Too many lines in module (3774>2000)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/133a1f19fc27e795b08dba47b15941dfac995e36/ibis/backends/tests/test_generic.py#L1'>ibis/backends/tests/test_generic.py:1:1:</a> PLC0302 Too many lines in module (2023>2000)
+ <a href='https://github.com/ibis-project/ibis/blob/133a1f19fc27e795b08dba47b15941dfac995e36/ibis/backends/tests/test_temporal.py#L1'>ibis/backends/tests/test_temporal.py:1:1:</a> PLC0302 Too many lines in module (2493>2000)
+ <a href='https://github.com/ibis-project/ibis/blob/133a1f19fc27e795b08dba47b15941dfac995e36/ibis/expr/api.py#L1'>ibis/expr/api.py:1:1:</a> PLC0302 Too many lines in module (2410>2000)
+ <a href='https://github.com/ibis-project/ibis/blob/133a1f19fc27e795b08dba47b15941dfac995e36/ibis/expr/types/generic.py#L1'>ibis/expr/types/generic.py:1:1:</a> PLC0302 Too many lines in module (2232>2000)
+ <a href='https://github.com/ibis-project/ibis/blob/133a1f19fc27e795b08dba47b15941dfac995e36/ibis/expr/types/relations.py#L1'>ibis/expr/types/relations.py:1:1:</a> PLC0302 Too many lines in module (4424>2000)
+ <a href='https://github.com/ibis-project/ibis/blob/133a1f19fc27e795b08dba47b15941dfac995e36/ibis/tests/expr/test_table.py#L1'>ibis/tests/expr/test_table.py:1:1:</a> PLC0302 Too many lines in module (2077>2000)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+54 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/conftest.py#L1'>pandas/conftest.py:1:1:</a> PLC0302 Too many lines in module (2027>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/arrays/arrow/array.py#L1'>pandas/core/arrays/arrow/array.py:1:1:</a> PLC0302 Too many lines in module (2981>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/arrays/base.py#L1'>pandas/core/arrays/base.py:1:1:</a> PLC0302 Too many lines in module (2604>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/arrays/categorical.py#L1'>pandas/core/arrays/categorical.py:1:1:</a> PLC0302 Too many lines in module (3122>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/arrays/datetimelike.py#L1'>pandas/core/arrays/datetimelike.py:1:1:</a> PLC0302 Too many lines in module (2615>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/arrays/datetimes.py#L1'>pandas/core/arrays/datetimes.py:1:1:</a> PLC0302 Too many lines in module (2835>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/dtypes/dtypes.py#L1'>pandas/core/dtypes/dtypes.py:1:1:</a> PLC0302 Too many lines in module (2353>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/frame.py#L1'>pandas/core/frame.py:1:1:</a> PLC0302 Too many lines in module (12924>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/generic.py#L1'>pandas/core/generic.py:1:1:</a> PLC0302 Too many lines in module (12875>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/groupby/generic.py#L1'>pandas/core/groupby/generic.py:1:1:</a> PLC0302 Too many lines in module (2814>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/groupby/groupby.py#L1'>pandas/core/groupby/groupby.py:1:1:</a> PLC0302 Too many lines in module (5623>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/indexes/base.py#L1'>pandas/core/indexes/base.py:1:1:</a> PLC0302 Too many lines in module (7513>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/indexes/multi.py#L1'>pandas/core/indexes/multi.py:1:1:</a> PLC0302 Too many lines in module (4099>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/indexing.py#L1'>pandas/core/indexing.py:1:1:</a> PLC0302 Too many lines in module (2778>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/internals/blocks.py#L1'>pandas/core/internals/blocks.py:1:1:</a> PLC0302 Too many lines in module (2371>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/internals/managers.py#L1'>pandas/core/internals/managers.py:1:1:</a> PLC0302 Too many lines in module (2524>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/resample.py#L1'>pandas/core/resample.py:1:1:</a> PLC0302 Too many lines in module (2703>2000)
+ <a href='https://github.com/pandas-dev/pandas/blob/41383cf140b8243613af8a9843448b54f2b3ffa8/pandas/core/reshape/merge.py#L1'>pandas/core/reshape/merge.py:1:1:</a> PLC0302 Too many lines in module (2751>2000)
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/api/rest.py#L1'>rotkehlchen/api/rest.py:1:1:</a> PLC0302 Too many lines in module (4799>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/api/v1/resources.py#L1'>rotkehlchen/api/v1/resources.py:1:1:</a> PLC0302 Too many lines in module (3199>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/api/v1/schemas.py#L1'>rotkehlchen/api/v1/schemas.py:1:1:</a> PLC0302 Too many lines in module (3466>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/db/dbhandler.py#L1'>rotkehlchen/db/dbhandler.py:1:1:</a> PLC0302 Too many lines in module (3559>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/globaldb/handler.py#L1'>rotkehlchen/globaldb/handler.py:1:1:</a> PLC0302 Too many lines in module (2120>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/tests/db/test_db_upgrades.py#L1'>rotkehlchen/tests/db/test_db_upgrades.py:1:1:</a> PLC0302 Too many lines in module (2496>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/tests/unit/decoders/test_makerdao_sai.py#L1'>rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1:1:</a> PLC0302 Too many lines in module (2487>2000)
+ <a href='https://github.com/rotki/rotki/blob/08e2e843a984e49d0820f964481a110b57a83806/rotkehlchen/tests/utils/dataimport.py#L1'>rotkehlchen/tests/utils/dataimport.py:1:1:</a> PLC0302 Too many lines in module (2516>2000)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+22 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/analytics/tests/test_counts.py#L1'>analytics/tests/test_counts.py:1:1:</a> PLC0302 Too many lines in module (2139>2000)
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/corporate/lib/stripe.py#L1'>corporate/lib/stripe.py:1:1:</a> PLC0302 Too many lines in module (5161>2000)
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/corporate/tests/test_stripe.py#L1'>corporate/tests/test_stripe.py:1:1:</a> PLC0302 Too many lines in module (9178>2000)
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/tools/setup/emoji/emoji_names.py#L1'>tools/setup/emoji/emoji_names.py:1:1:</a> PLC0302 Too many lines in module (2081>2000)
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/zerver/actions/message_send.py#L1'>zerver/actions/message_send.py:1:1:</a> PLC0302 Too many lines in module (2008>2000)
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/zerver/lib/export.py#L1'>zerver/lib/export.py:1:1:</a> PLC0302 Too many lines in module (2443>2000)
... 16 additional changes omitted for rule PLC0302
+ <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/zerver/lib/test_classes.py#L1'>zerver/lib/test_classes.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/zulip/zulip/blob/75411b264e3a7d9e8fb01b6195acc78a92f49ca2/zerver/lib/test_classes.py#L1'>zerver/lib/test_classes.py:1:1:</a> D100 Missing docstring in public module
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0302 | 141 | 141 | 0 | 0 | 0 |
| D100 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @augustelalande on 2024-03-07 02:28_

~~Not sure if docs should be considered a KNOWN_FORMATTING_VIOLATIONS, or formatting should be changed. I did my best to copy the docs from the [pylint docs](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/too-many-lines.html).~~

Nevermind

---

_Comment by @eslamodeh on 2024-03-07 14:36_

Would be great to have this merged 🙌 🙌 

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:74 on 2024-03-21 21:41_

Could it also show the number? e.g. pylint shows `Too many lines in module (3000/2000)`
I think use `>` sign is more proper there:
`Too many lines in module (3000>2000)`

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:83 on 2024-03-21 21:44_

```suggestion
    let lines_count = locator.contents().universal_newlines().count();

    if lines_count > settings.pylint.max_module_lines {
```
Is there any reason that you increased the lines count by one?

---

_@T-256 reviewed on 2024-03-21 21:44_

---

_@augustelalande reviewed on 2024-03-21 21:54_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:83 on 2024-03-21 21:54_

universal_newlines splits on  newline characters, so if the last line is empty it won't be counted. Really what I want to count is the number of newline characters, but I didn't find anything to do that.

It's a good point though that if the last line is not empty, then this will be off-by-one in the other direction.

---

_@augustelalande reviewed on 2024-03-21 22:02_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:74 on 2024-03-21 22:02_

Done

---

_@T-256 reviewed on 2024-03-21 22:12_

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:83 on 2024-03-21 22:12_

Correct, it should increase by one when the last line is not empty.

I think lines count of all below contents is **1**:
- `first`
- `first\n`
- `first\r\n`

In this PR, `first\n` consumed as 2 lines, I'm not sure if it is expected behavior.

---

_@augustelalande reviewed on 2024-03-21 22:13_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:83 on 2024-03-21 22:13_

I'll check against pylint

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:83 on 2024-03-21 22:24_

Ok pylint doesn't count the final newline, but it does count multiple final new lines, so `first\n\n` should be 2 but `first\n` should be one. I will update it.

---

_@augustelalande reviewed on 2024-03-21 22:24_

---

_@augustelalande reviewed on 2024-03-21 22:35_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pylint/rules/too_many_lines.rs`:83 on 2024-03-21 22:35_

Done

---

_Comment by @MichaReiser on 2024-03-22 07:38_

Thanks for your contribution. Unfortunately, the rule is incompatible with the formatter because the formatter can introduce new violations by splitting lines across multiple lines, making the module exceed the configured max lines. We currently want to hold off from adding new rules that conflict with the formatter because everyone using `--select ALL` automatically opts into these rules, making ruff print an ugly warning that the rules are incompatible. We can reconsider adding this rule after we've finished #1774. 

I'm sorry we didn't point this out on the PyLint issue before you started implementing it. This must feel frustrating to you and we could have done better. I'm trying to catch up with rules and plan to first go through the open PRs and later triage open issues to ensure we communicate clearly which rules will be accepted today.

---

_Closed by @MichaReiser on 2024-03-22 07:38_

---

_Comment by @augustelalande on 2024-03-22 14:08_

@MichaReiser No worries. I do hope we can merge this eventually though, because I think it is an important check.

---

_Branch deleted on 2024-03-31 15:59_

---
