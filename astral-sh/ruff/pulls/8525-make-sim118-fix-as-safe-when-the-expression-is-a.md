```yaml
number: 8525
title: "Make `SIM118` fix as safe when the expression is a known dictionary "
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: charlie/key-in
created_at: 2023-11-06T20:14:52Z
updated_at: 2023-11-06T21:14:30Z
url: https://github.com/astral-sh/ruff/pull/8525
synced_at: 2026-01-10T23:40:55Z
```

# Make `SIM118` fix as safe when the expression is a known dictionary 

---

_Pull request opened by @charliermarsh on 2023-11-06 20:14_

## Summary

Given `key in obj.keys()`, `obj` _could_ be a dictionary, or it could be another type that defines
a `.keys()` method. In the latter case, removing the `.keys()` attribute
could lead to a runtime error.

Previously, we marked all `SIM118` fixes as unsafe for this reason; however, in preview, we now mark them as safe if we can
infer that the expression is a dictionary.

## Test Plan

Added a preview fixture.


---

_Label `autofix` added by @charliermarsh on 2023-11-06 20:15_

---

_Label `preview` added by @charliermarsh on 2023-11-06 20:15_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-06 20:15_

---

_@zanieb reviewed on 2023-11-06 20:54_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs`:43 on 2023-11-06 20:54_

How does this render?

---

_@zanieb approved on 2023-11-06 20:55_

---

_@charliermarsh reviewed on 2023-11-06 21:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs`:43 on 2023-11-06 21:00_

Fixed, thanks.

---

_Merged by @charliermarsh on 2023-11-06 21:06_

---

_Closed by @charliermarsh on 2023-11-06 21:06_

---

_Branch deleted on 2023-11-06 21:06_

---

_Comment by @github-actions[bot] on 2023-11-06 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +380 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +20 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/providers/google/cloud/utils/datafusion.py#L33'>airflow/providers/google/cloud/utils/datafusion.py:33:79:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/providers/google/cloud/utils/datafusion.py#L33'>airflow/providers/google/cloud/utils/datafusion.py:33:79:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/dev/breeze/src/airflow_breeze/utils/provider_dependencies.py#L68'>dev/breeze/src/airflow_breeze/utils/provider_dependencies.py:68:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/dev/breeze/src/airflow_breeze/utils/provider_dependencies.py#L68'>dev/breeze/src/airflow_breeze/utils/provider_dependencies.py:68:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L765'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:765:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L765'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:765:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L805'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:805:24:</a> SIM118 Use `key not in dict` instead of `key not in dict.keys()`
+ <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L805'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:805:24:</a> SIM118 [*] Use `key not in dict` instead of `key not in dict.keys()`
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L807'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:807:24:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py#L807'>tests/providers/amazon/aws/executors/ecs/test_ecs_executor.py:807:24:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L108'>tests/unit/bokeh/models/test_sources.py:108:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L108'>tests/unit/bokeh/models/test_sources.py:108:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L120'>tests/unit/bokeh/models/test_sources.py:120:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L120'>tests/unit/bokeh/models/test_sources.py:120:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L134'>tests/unit/bokeh/models/test_sources.py:134:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L134'>tests/unit/bokeh/models/test_sources.py:134:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L82'>tests/unit/bokeh/models/test_sources.py:82:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L82'>tests/unit/bokeh/models/test_sources.py:82:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L96'>tests/unit/bokeh/models/test_sources.py:96:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/models/test_sources.py#L96'>tests/unit/bokeh/models/test_sources.py:96:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +350 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L921'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:921:12:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L921'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:921:12:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Absolute/Integrations/Absolute/Absolute.py#L331'>Packs/Absolute/Integrations/Absolute/Absolute.py:331:8:</a> SIM118 Use `key not in dict` instead of `key not in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Absolute/Integrations/Absolute/Absolute.py#L331'>Packs/Absolute/Integrations/Absolute/Absolute.py:331:8:</a> SIM118 [*] Use `key not in dict` instead of `key not in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AbuseDB/Integrations/AbuseDB/AbuseDB.py#L144'>Packs/AbuseDB/Integrations/AbuseDB/AbuseDB.py:144:57:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AbuseDB/Integrations/AbuseDB/AbuseDB.py#L144'>Packs/AbuseDB/Integrations/AbuseDB/AbuseDB.py:144:57:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AlienVault_OTX/Integrations/AlienVault_OTX_v2/AlienVault_OTX_v2.py#L282'>Packs/AlienVault_OTX/Integrations/AlienVault_OTX_v2/AlienVault_OTX_v2.py:282:46:</a> SIM118 Use `key not in dict` instead of `key not in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AlienVault_OTX/Integrations/AlienVault_OTX_v2/AlienVault_OTX_v2.py#L282'>Packs/AlienVault_OTX/Integrations/AlienVault_OTX_v2/AlienVault_OTX_v2.py:282:46:</a> SIM118 [*] Use `key not in dict` instead of `key not in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py#L383'>Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py:383:40:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py#L383'>Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py:383:40:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py#L777'>Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py:777:47:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py#L777'>Packs/Anomali_ThreatStream/Integrations/Anomali_ThreatStream_v2/Anomali_ThreatStream_v2.py:777:47:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/ApiModules/Scripts/HTTPFeedApiModule/HTTPFeedApiModule.py#L291'>Packs/ApiModules/Scripts/HTTPFeedApiModule/HTTPFeedApiModule.py:291:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/ApiModules/Scripts/HTTPFeedApiModule/HTTPFeedApiModule.py#L291'>Packs/ApiModules/Scripts/HTTPFeedApiModule/HTTPFeedApiModule.py:291:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L157'>Packs/AppNovi/Integrations/appNovi/appNovi.py:157:31:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L157'>Packs/AppNovi/Integrations/appNovi/appNovi.py:157:31:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L201'>Packs/AppNovi/Integrations/appNovi/appNovi.py:201:31:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L201'>Packs/AppNovi/Integrations/appNovi/appNovi.py:201:31:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L249'>Packs/AppNovi/Integrations/appNovi/appNovi.py:249:46:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L249'>Packs/AppNovi/Integrations/appNovi/appNovi.py:249:46:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L288'>Packs/AppNovi/Integrations/appNovi/appNovi.py:288:46:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L288'>Packs/AppNovi/Integrations/appNovi/appNovi.py:288:46:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L353'>Packs/AppNovi/Integrations/appNovi/appNovi.py:353:50:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AppNovi/Integrations/appNovi/appNovi.py#L353'>Packs/AppNovi/Integrations/appNovi/appNovi.py:353:50:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py#L533'>Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py:533:12:</a> SIM118 Use `key not in dict` instead of `key not in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py#L533'>Packs/AtlassianConfluenceCloud/Integrations/AtlassianConfluenceCloud/AtlassianConfluenceCloud.py:533:12:</a> SIM118 [*] Use `key not in dict` instead of `key not in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Aws-SecretsManager/Integrations/AwsSecretsManager/AwsSecretsManager.py#L235'>Packs/Aws-SecretsManager/Integrations/AwsSecretsManager/AwsSecretsManager.py:235:9:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Aws-SecretsManager/Integrations/AwsSecretsManager/AwsSecretsManager.py#L235'>Packs/Aws-SecretsManager/Integrations/AwsSecretsManager/AwsSecretsManager.py:235:9:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BPA/Integrations/BPA/BPA.py#L125'>Packs/BPA/Integrations/BPA/BPA.py:125:9:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BPA/Integrations/BPA/BPA.py#L125'>Packs/BPA/Integrations/BPA/BPA.py:125:9:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L566'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:566:12:</a> SIM118 Use `key not in dict` instead of `key not in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L566'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:566:12:</a> SIM118 [*] Use `key not in dict` instead of `key not in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L707'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:707:9:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py#L707'>Packs/Base/Scripts/DBotFindSimilarIncidents/DBotFindSimilarIncidents.py:707:9:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L241'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:241:50:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L241'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:241:50:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L242'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:242:20:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L242'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:242:20:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L259'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:259:78:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L259'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:259:78:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1025'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1025:9:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1025'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1025:9:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1413'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1413:37:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1413'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1413:37:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
- <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1426'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1426:10:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/demisto/content/blob/f706ff906050b69bd4a7f613b1257203ff9bd13d/Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py#L1426'>Packs/BmcHelixRemedyForce/Integrations/BmcHelixRemedyForce/BmcHelixRemedyForce.py:1426:10:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
... 304 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM118 | 380 | 0 | 0 | 380 | 0 |

</p>
</details>




---
