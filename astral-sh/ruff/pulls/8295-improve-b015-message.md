```yaml
number: 8295
title: Improve B015 message
type: pull_request
state: merged
author: harupy
labels:
  - documentation
assignees: []
merged: true
base: main
head: improve-B015-message
created_at: 2023-10-28T03:49:48Z
updated_at: 2023-10-28T11:31:13Z
url: https://github.com/astral-sh/ruff/pull/8295
synced_at: 2026-01-12T02:32:42Z
```

# Improve B015 message

---

_Pull request opened by @harupy on 2023-10-28 03:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #8069

## Test Plan

<!-- How was it tested? -->

cargo test


---

_@charliermarsh reviewed on 2023-10-28 03:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_comparison.rs`:36 on 2023-10-28 03:57_

I think this should now be `Otherwise, prepend...`

---

_@charliermarsh reviewed on 2023-10-28 03:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_comparison.rs`:35 on 2023-10-28 03:58_

I think I'd suggest removing "This comparison does nothing.", it feels redundant.

---

_Comment by @charliermarsh on 2023-10-28 03:58_

Thanks @harupy!

---

_Comment by @github-actions[bot] on 2023-10-28 04:06_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected linter changes**. (+67 -67 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+35 -35 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/core/test_otel_logger.py#L179'>tests/core/test_otel_logger.py:179:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/core/test_otel_logger.py#L179'>tests/core/test_otel_logger.py:179:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/core/test_otel_logger.py#L198'>tests/core/test_otel_logger.py:198:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/core/test_otel_logger.py#L198'>tests/core/test_otel_logger.py:198:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/core/test_otel_logger.py#L253'>tests/core/test_otel_logger.py:253:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/core/test_otel_logger.py#L253'>tests/core/test_otel_logger.py:253:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/decorators/test_mapped.py#L37'>tests/decorators/test_mapped.py:37:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/decorators/test_mapped.py#L37'>tests/decorators/test_mapped.py:37:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/decorators/test_mapped.py#L38'>tests/decorators/test_mapped.py:38:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/decorators/test_mapped.py#L38'>tests/decorators/test_mapped.py:38:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_backfill_job.py#L1999'>tests/jobs/test_backfill_job.py:1999:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_backfill_job.py#L1999'>tests/jobs/test_backfill_job.py:1999:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_backfill_job.py#L2000'>tests/jobs/test_backfill_job.py:2000:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_backfill_job.py#L2000'>tests/jobs/test_backfill_job.py:2000:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_backfill_job.py#L2001'>tests/jobs/test_backfill_job.py:2001:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_backfill_job.py#L2001'>tests/jobs/test_backfill_job.py:2001:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_scheduler_job.py#L1906'>tests/jobs/test_scheduler_job.py:1906:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_scheduler_job.py#L1906'>tests/jobs/test_scheduler_job.py:1906:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_scheduler_job.py#L3946'>tests/jobs/test_scheduler_job.py:3946:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/jobs/test_scheduler_job.py#L3946'>tests/jobs/test_scheduler_job.py:3946:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/models/test_baseoperator.py#L648'>tests/models/test_baseoperator.py:648:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/models/test_baseoperator.py#L648'>tests/models/test_baseoperator.py:648:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/models/test_baseoperator.py#L656'>tests/models/test_baseoperator.py:656:13:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/models/test_baseoperator.py#L656'>tests/models/test_baseoperator.py:656:13:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/models/test_baseoperator.py#L658'>tests/models/test_baseoperator.py:658:13:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/apache/airflow/blob/6f3d294645153db914be69cd2b2a49f12a18280c/tests/models/test_baseoperator.py#L658'>tests/models/test_baseoperator.py:658:13:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/document/test_callbacks__document.py#L314'>tests/unit/bokeh/document/test_callbacks__document.py:314:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/document/test_callbacks__document.py#L314'>tests/unit/bokeh/document/test_callbacks__document.py:314:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/document/test_callbacks__document.py#L317'>tests/unit/bokeh/document/test_callbacks__document.py:317:9:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/document/test_callbacks__document.py#L317'>tests/unit/bokeh/document/test_callbacks__document.py:317:9:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/models/test_plots.py#L485'>tests/unit/bokeh/models/test_plots.py:485:13:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/tests/unit/bokeh/models/test_plots.py#L485'>tests/unit/bokeh/models/test_plots.py:485:13:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+29 -29 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/AzureSecurityCenter/Integrations/MicrosoftDefenderForCloudEventCollector/MicrosoftDefenderForCloudEventCollector_test.py#L188'>Packs/AzureSecurityCenter/Integrations/MicrosoftDefenderForCloudEventCollector/MicrosoftDefenderForCloudEventCollector_test.py:188:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/AzureSecurityCenter/Integrations/MicrosoftDefenderForCloudEventCollector/MicrosoftDefenderForCloudEventCollector_test.py#L188'>Packs/AzureSecurityCenter/Integrations/MicrosoftDefenderForCloudEventCollector/MicrosoftDefenderForCloudEventCollector_test.py:188:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py#L434'>Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py:434:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py#L434'>Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py:434:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py#L503'>Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py:503:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py#L503'>Packs/AzureWAF/Integrations/AzureWAF/AzureWAF_test.py:503:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L157'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:157:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L157'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:157:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L159'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:159:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L159'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:159:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L165'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:165:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L165'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:165:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L166'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:166:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L166'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:166:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L167'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:167:5:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py#L167'>Packs/Base/Scripts/DBotSuggestClassifierMapping/dbot_suggest_classifier_mapping_test.py:167:5:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/CTIX/Integrations/CTIX/CTIX.py#L492'>Packs/CTIX/Integrations/CTIX/CTIX.py:492:17:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/CTIX/Integrations/CTIX/CTIX.py#L492'>Packs/CTIX/Integrations/CTIX/CTIX.py:492:17:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/CTIX/Integrations/CTIX/CTIX.py#L520'>Packs/CTIX/Integrations/CTIX/CTIX.py:520:17:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
- <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/CTIX/Integrations/CTIX/CTIX.py#L520'>Packs/CTIX/Integrations/CTIX/CTIX.py:520:17:</a> B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.
+ <a href='https://github.com/demisto/content/blob/cc4f0426702458e6c833747164229add8ea12797/Packs/CTIX/Integrations/CTIXv3/CTIXv3.py#L808'>Packs/CTIX/Integrations/CTIXv3/CTIXv3.py:808:21:</a> B015 Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B015 | 134 | 67 | 67 | 0 | 0 |

</p>
</details>




---

_@harupy reviewed on 2023-10-28 04:11_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_bugbear/rules/useless_comparison.rs`:35 on 2023-10-28 04:11_

@charliermarsh should `or remove it` be kept?

---

_Merged by @charliermarsh on 2023-10-28 11:18_

---

_Closed by @charliermarsh on 2023-10-28 11:18_

---

_Label `documentation` added by @charliermarsh on 2023-10-28 11:18_

---

_Branch deleted on 2023-10-28 11:31_

---
