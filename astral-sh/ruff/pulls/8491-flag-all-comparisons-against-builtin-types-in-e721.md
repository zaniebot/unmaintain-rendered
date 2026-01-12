```yaml
number: 8491
title: Flag all comparisons against builtin types in E721
type: pull_request
state: merged
author: hauntsaninja
labels:
  - rule
assignees: []
merged: true
base: main
head: all-builtin-types
created_at: 2023-11-05T05:15:14Z
updated_at: 2023-11-06T03:54:33Z
url: https://github.com/astral-sh/ruff/pull/8491
synced_at: 2026-01-10T23:40:55Z
```

# Flag all comparisons against builtin types in E721

---

_Pull request opened by @hauntsaninja on 2023-11-05 05:15_

See #8483. Generalised fix on top of #8485

Based on the output of `print("\n".join(k for k, v in builtins.__dict__.items() if isinstance(v, type)))`

---

_Comment by @github-actions[bot] on 2023-11-05 05:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+222 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/.github/tests/test_validate_gpus.py#L26'>.github/tests/test_validate_gpus.py:26:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/utils/io.py#L66'>rasa/shared/utils/io.py:66:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/utils/io.py#L68'>rasa/shared/utils/io.py:68:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/utils/io.py#L93'>rasa/shared/utils/io.py:93:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/cli/test_utils.py#L513'>tests/cli/test_utils.py:513:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/entityset/entityset.py#L402'>featuretools/entityset/entityset.py:402:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/539a48413387bc1bf300f67c7a567c2c42da41d1/tests/core/test_configuration.py#L1448'>tests/core/test_configuration.py:1448:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/539a48413387bc1bf300f67c7a567c2c42da41d1/tests/core/test_configuration.py#L1458'>tests/core/test_configuration.py:1458:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/539a48413387bc1bf300f67c7a567c2c42da41d1/tests/core/test_configuration.py#L1482'>tests/core/test_configuration.py:1482:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/539a48413387bc1bf300f67c7a567c2c42da41d1/tests/core/test_configuration.py#L1487'>tests/core/test_configuration.py:1487:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/examples/server/app/crossfilter/main.py#L32'>examples/server/app/crossfilter/main.py:32:35:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/bokeh/bokeh/blob/53ec08ae83137bfbd168f1efd64fcc3161c0a964/tests/unit/bokeh/models/test_sources.py#L288'>tests/unit/bokeh/models/test_sources.py:288:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/AHA/Integrations/AHA/AHA_test.py#L89'>Packs/AHA/Integrations/AHA/AHA_test.py:89:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/Campaign/Scripts/CollectCampaignRecipients/CollectCampaignRecipients_test.py#L82'>Packs/Campaign/Scripts/CollectCampaignRecipients/CollectCampaignRecipients_test.py:82:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/CommonScripts/Scripts/FindSimilarIncidentsV2/find_similar_incidents_test.py#L252'>Packs/CommonScripts/Scripts/FindSimilarIncidentsV2/find_similar_incidents_test.py:252:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/CommonScripts/Scripts/FindSimilarIncidentsV2/find_similar_incidents_test.py#L280'>Packs/CommonScripts/Scripts/FindSimilarIncidentsV2/find_similar_incidents_test.py:280:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/Mattermost/Scripts/MattermostAskUser/MattermostAskUser_test.py#L23'>Packs/Mattermost/Scripts/MattermostAskUser/MattermostAskUser_test.py:23:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/MicrosoftTeams/Integrations/MicrosoftTeams/MicrosoftTeams_test.py#L2029'>Packs/MicrosoftTeams/Integrations/MicrosoftTeams/MicrosoftTeams_test.py:2029:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/MicrosoftTeams/Integrations/MicrosoftTeams/MicrosoftTeams_test.py#L2102'>Packs/MicrosoftTeams/Integrations/MicrosoftTeams/MicrosoftTeams_test.py:2102:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/ServiceDeskPlus/Integrations/ServiceDeskPlus/ServiceDeskPlus_test.py#L156'>Packs/ServiceDeskPlus/Integrations/ServiceDeskPlus/ServiceDeskPlus_test.py:156:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/TheHiveProject/Integrations/TheHiveProject/TheHiveProject.py#L504'>Packs/TheHiveProject/Integrations/TheHiveProject/TheHiveProject.py:504:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/demisto/content/blob/6a3e76659345bed9437077b938525d45377e9c08/Packs/TheHiveProject/Integrations/TheHiveProject/TheHiveProject.py#L553'>Packs/TheHiveProject/Integrations/TheHiveProject/TheHiveProject.py:553:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/2e3a5a0d8836e94b6e7a7495b11438f8fe2d5f85/ibis/backends/bigquery/tests/system/test_client.py#L228'>ibis/backends/bigquery/tests/system/test_client.py:228:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/2e3a5a0d8836e94b6e7a7495b11438f8fe2d5f85/ibis/backends/bigquery/tests/system/test_client.py#L229'>ibis/backends/bigquery/tests/system/test_client.py:229:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/2e3a5a0d8836e94b6e7a7495b11438f8fe2d5f85/ibis/backends/tests/test_client.py#L1104'>ibis/backends/tests/test_client.py:1104:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ing-bank/probatus">ing-bank/probatus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ing-bank/probatus/blob/98e3329af770fc4ca188d471b0237057328d00af/probatus/stat_tests/distribution_statistics.py#L192'>probatus/stat_tests/distribution_statistics.py:192:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+191 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/_testing/_warnings.py#L172'>pandas/_testing/_warnings.py:172:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/_numba/extensions.py#L51'>pandas/core/_numba/extensions.py:51:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/algorithms.py#L526'>pandas/core/algorithms.py:526:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/algorithms.py#L783'>pandas/core/algorithms.py:783:36:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/algorithms.py#L933'>pandas/core/algorithms.py:933:38:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/array_algos/putmask.py#L138'>pandas/core/array_algos/putmask.py:138:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/array_algos/putmask.py#L44'>pandas/core/array_algos/putmask.py:44:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/array_algos/take.py#L378'>pandas/core/array_algos/take.py:378:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/datetimelike.py#L451'>pandas/core/arrays/datetimelike.py:451:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/datetimelike.py#L655'>pandas/core/arrays/datetimelike.py:655:40:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/datetimelike.py#L766'>pandas/core/arrays/datetimelike.py:766:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/datetimes.py#L2236'>pandas/core/arrays/datetimes.py:2236:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/datetimes.py#L2423'>pandas/core/arrays/datetimes.py:2423:10:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/masked.py#L483'>pandas/core/arrays/masked.py:483:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/masked.py#L939'>pandas/core/arrays/masked.py:939:30:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/numeric.py#L161'>pandas/core/arrays/numeric.py:161:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/numpy_.py#L134'>pandas/core/arrays/numpy_.py:134:35:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/string_arrow.py#L630'>pandas/core/arrays/string_arrow.py:630:32:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/timedeltas.py#L1063'>pandas/core/arrays/timedeltas.py:1063:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/arrays/timedeltas.py#L672'>pandas/core/arrays/timedeltas.py:672:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/computation/expr.py#L523'>pandas/core/computation/expr.py:523:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/computation/expr.py#L524'>pandas/core/computation/expr.py:524:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/computation/ops.py#L242'>pandas/core/computation/ops.py:242:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/construction.py#L602'>pandas/core/construction.py:602:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/construction.py#L642'>pandas/core/construction.py:642:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/construction.py#L775'>pandas/core/construction.py:775:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/astype.py#L103'>pandas/core/dtypes/astype.py:103:10:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/astype.py#L131'>pandas/core/dtypes/astype.py:131:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/astype.py#L131'>pandas/core/dtypes/astype.py:131:39:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/common.py#L518'>pandas/core/dtypes/common.py:518:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/dtypes.py#L1422'>pandas/core/dtypes/dtypes.py:1422:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/dtypes.py#L1794'>pandas/core/dtypes/dtypes.py:1794:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/dtypes.py#L450'>pandas/core/dtypes/dtypes.py:450:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/dtypes/missing.py#L621'>pandas/core/dtypes/missing.py:621:10:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/frame.py#L11404'>pandas/core/frame.py:11404:15:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/frame.py#L11406'>pandas/core/frame.py:11406:33:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/frame.py#L2427'>pandas/core/frame.py:2427:24:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/frame.py#L7985'>pandas/core/frame.py:7985:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/frame.py#L8012'>pandas/core/frame.py:8012:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/generic.py#L8245'>pandas/core/generic.py:8245:23:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/generic.py#L8247'>pandas/core/generic.py:8247:50:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/generic.py#L8271'>pandas/core/generic.py:8271:37:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/6493d2a4bcf402d5fcb8b5349e70928d8dbf3344/pandas/core/groupby/groupby.py#L1932'>pandas/core/groupby/groupby.py:1932:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
... 148 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E721 | 222 | 222 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @hauntsaninja on 2023-11-05 05:43_

---

_Comment by @charliermarsh on 2023-11-05 14:50_

Nice, thanks -- this looks reasonable to me.

---

_Marked ready for review by @hauntsaninja on 2023-11-05 19:43_

---

_Converted to draft by @hauntsaninja on 2023-11-05 19:44_

---

_Marked ready for review by @hauntsaninja on 2023-11-05 19:47_

---

_Comment by @charliermarsh on 2023-11-06 01:01_

I'm wondering if this should be part of preview. What do you think @zanieb? It doesn't change the intent of the rule, but it will lead to more violations. (And for some of these, there isn't a great workaround right now, since we don't accept `type(foo) is object` -- which we _do_ accept in preview.)

---

_Label `rule` added by @charliermarsh on 2023-11-06 01:01_

---

_Comment by @hauntsaninja on 2023-11-06 01:43_

I'd be in favour of only landing this change in stable once #7905 is in stable

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/type_comparison.rs`:222 on 2023-11-06 01:51_

Let's revert this change (but keep the change in the other `matches!` below).

---

_@charliermarsh reviewed on 2023-11-06 01:51_

---

_Comment by @charliermarsh on 2023-11-06 01:52_

@hauntsaninja -- Yeah same. That would be equivalent to reverting this change in the `deprecated_*` variant.

---

_@charliermarsh approved on 2023-11-06 02:28_

Thanks!

---

_Merged by @charliermarsh on 2023-11-06 02:28_

---

_Closed by @charliermarsh on 2023-11-06 02:28_

---

_Branch deleted on 2023-11-06 03:54_

---
