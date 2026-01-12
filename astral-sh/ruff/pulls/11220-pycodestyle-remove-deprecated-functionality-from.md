```yaml
number: 11220
title: "[`pycodestyle`] Remove deprecated functionality from `type-comparison` (`E721`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: e721
created_at: 2024-04-30T19:12:55Z
updated_at: 2024-06-25T17:14:00Z
url: https://github.com/astral-sh/ruff/pull/11220
synced_at: 2026-01-12T15:55:37Z
```

# [`pycodestyle`] Remove deprecated functionality from `type-comparison` (`E721`)

---

_@augustelalande_

## Summary

Stabilizes `E721` behavior implemented in #7905.

The functionality change in `E721` was implemented in #7905, released in [v0.1.2](https://github.com/astral-sh/ruff/releases/tag/v0.1.2). And seems functionally stable since #9676, without an explicit release but would correspond to [v0.2.0](https://github.com/astral-sh/ruff/releases/tag/v0.2.0). So the deprecated functionally should be removable in the next minor release.

resolves: #6465 

---

_Comment by @zanieb on 2024-04-30 19:18_

To clarify, this looks like it stabilizes the preview behavior introduced in https://github.com/astral-sh/ruff/pull/9676 correct?

---

_Added to milestone `v0.5.0` by @zanieb on 2024-04-30 19:18_

---

_Comment by @augustelalande on 2024-04-30 19:21_

Yes, sorry I wasn't clear. I updated the PR summary accordingly.

---

_Comment by @github-actions[bot] on 2024-04-30 19:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+313 -251 violations, +0 -0 fixes in 19 projects; 25 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/disnake/ext/commands/params.py#L807'>disnake/ext/commands/params.py:807:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_params.py#L143'>tests/ext/commands/test_params.py:143:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_params.py#L146'>tests/ext/commands/test_params.py:146:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/tests/ext/commands/test_params.py#L214'>tests/ext/commands/test_params.py:214:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
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
+ <a href='https://github.com/alteryx/featuretools/blob/21d0bf0915238ba6c6bc1e958b9a91b209f88de5/featuretools/tests/entityset_tests/test_es.py#L860'>featuretools/tests/entityset_tests/test_es.py:860:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/alteryx/featuretools/blob/21d0bf0915238ba6c6bc1e958b9a91b209f88de5/featuretools/tests/entityset_tests/test_serialization.py#L134'>featuretools/tests/entityset_tests/test_serialization.py:134:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/alteryx/featuretools/blob/21d0bf0915238ba6c6bc1e958b9a91b209f88de5/featuretools/tests/entityset_tests/test_serialization.py#L135'>featuretools/tests/entityset_tests/test_serialization.py:135:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0d781602cfd4a43281e0807b2edcbbcaa00c574b/tests/particles/test_exceptions.py#L1040'>tests/particles/test_exceptions.py:1040:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/airflow/models/abstractoperator.py#L425'>airflow/models/abstractoperator.py:425:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/airflow/models/abstractoperator.py#L427'>airflow/models/abstractoperator.py:427:14:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/airflow/models/abstractoperator.py#L429'>airflow/models/abstractoperator.py:429:14:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/tests/core/test_configuration.py#L1487'>tests/core/test_configuration.py:1487:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/tests/core/test_configuration.py#L1497'>tests/core/test_configuration.py:1497:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/tests/core/test_configuration.py#L1521'>tests/core/test_configuration.py:1521:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/tests/core/test_configuration.py#L1526'>tests/core/test_configuration.py:1526:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/tests/core/test_stats.py#L387'>tests/core/test_stats.py:387:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/apache/airflow/blob/10e34d348e15666a7d231bfbca2d1ffd555e7c63/tests/core/test_stats.py#L394'>tests/core/test_stats.py:394:16:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_primitive.py#L249'>tests/unit/bokeh/core/property/test_primitive.py:249:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L288'>tests/unit/bokeh/models/test_sources.py:288:20:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+124 -231 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AHA/Integrations/AHA/AHA_test.py#L89'>Packs/AHA/Integrations/AHA/AHA_test.py:89:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L355'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:355:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L355'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:355:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L356'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:356:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L356'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:356:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L357'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:357:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L357'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:357:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L358'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:358:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L358'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:358:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L359'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:359:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py#L359'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector_test.py:359:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-IAM/Integrations/AWS-IAM/AWS-IAM_test.py#L433'>Packs/AWS-IAM/Integrations/AWS-IAM/AWS-IAM_test.py:433:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-IAM/Integrations/AWS-IAM/AWS-IAM_test.py#L433'>Packs/AWS-IAM/Integrations/AWS-IAM/AWS-IAM_test.py:433:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub_test.py#L323'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub_test.py:323:12:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub_test.py#L323'>Packs/AWS-SecurityHub/Integrations/AWS_SecurityHub/AWS_SecurityHub_test.py:323:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS_Sagemaker/Integrations/AWSSagemaker/AWSSagemaker.py#L52'>Packs/AWS_Sagemaker/Integrations/AWSSagemaker/AWSSagemaker.py:52:8:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AWS_Sagemaker/Integrations/AWSSagemaker/AWSSagemaker.py#L52'>Packs/AWS_Sagemaker/Integrations/AWSSagemaker/AWSSagemaker.py:52:8:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AbuseDB/Integrations/AbuseDB/AbuseDB.py#L323'>Packs/AbuseDB/Integrations/AbuseDB/AbuseDB.py:323:24:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L148'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:148:17:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L171'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:171:17:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L207'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:207:17:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py#L244'>Packs/AcalvioShadowplex/Integrations/acalvioapp/acalvioapp.py:244:17:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AccentureCTI/Integrations/ACTIIndicatorQuery/ACTIIndicatorQuery.py#L230'>Packs/AccentureCTI/Integrations/ACTIIndicatorQuery/ACTIIndicatorQuery.py:230:8:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/AccentureCTI/Integrations/ACTIIndicatorQuery/ACTIIndicatorQuery.py#L234'>Packs/AccentureCTI/Integrations/ACTIIndicatorQuery/ACTIIndicatorQuery.py:234:8:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L36'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:36:13:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L36'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:36:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L36'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:36:40:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L36'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:36:40:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L37'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:37:17:</a> E721 Do not compare types, use `isinstance()`
+ <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L37'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:37:17:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
- <a href='https://github.com/demisto/content/blob/638737044aa1b318b8ac5f57b3397fb53d6bda59/Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py#L43'>Packs/Active_Directory_Query/Scripts/SendEmailToManager/SendEmailToManager.py:43:42:</a> E721 Do not compare types, use `isinstance()`
... 324 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/admin/securedrop_admin/__init__.py#L588'>admin/securedrop_admin/__init__.py:588:12:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/admin/securedrop_admin/__init__.py#L590'>admin/securedrop_admin/__init__.py:590:12:</a> E721 Do not compare types, use `isinstance()`
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/admin/securedrop_admin/__init__.py#L594'>admin/securedrop_admin/__init__.py:594:12:</a> E721 Do not compare types, use `isinstance()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/backends/dask/convert.py#L28'>ibis/backends/dask/convert.py:28:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/backends/pandas/convert.py#L28'>ibis/backends/pandas/convert.py:28:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/backends/tests/test_client.py#L1094'>ibis/backends/tests/test_client.py:1094:13:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/tests/expr/test_sql_builtins.py#L116'>ibis/tests/expr/test_sql_builtins.py:116:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/tests/expr/test_sql_builtins.py#L117'>ibis/tests/expr/test_sql_builtins.py:117:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/tests/expr/test_sql_builtins.py#L137'>ibis/tests/expr/test_sql_builtins.py:137:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/tests/expr/test_sql_builtins.py#L174'>ibis/tests/expr/test_sql_builtins.py:174:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/ibis-project/ibis/blob/63dcb92bc1372832c61591bf95ae3ed0b89543e6/ibis/tests/expr/test_sql_builtins.py#L90'>ibis/tests/expr/test_sql_builtins.py:90:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E721 | 564 | 313 | 251 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-04-30 19:27_

Thanks this makes sense to me but we need to wait until 0.5.0 to merge.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-25 05:56_

---

_@dhruvmanila approved on 2024-06-25 16:17_

The ecosystem changes looks good to me. Thanks for doing this.

---

_Label `rule` added by @dhruvmanila on 2024-06-25 16:19_

---

_Merged by @dhruvmanila on 2024-06-25 16:21_

---

_Closed by @dhruvmanila on 2024-06-25 16:21_

---

_Branch deleted on 2024-06-25 17:14_

---
