```yaml
number: 11255
title: "Rephrase rationale for `pytest-incorrect-pytest-import`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pt
created_at: 2024-05-03T00:44:58Z
updated_at: 2024-05-03T00:57:11Z
url: https://github.com/astral-sh/ruff/pull/11255
synced_at: 2026-01-10T22:37:02Z
```

# Rephrase rationale for `pytest-incorrect-pytest-import`

---

_Pull request opened by @charliermarsh on 2024-05-03 00:44_

## Summary

Closes https://github.com/astral-sh/ruff/issues/11247.


---

_Label `documentation` added by @charliermarsh on 2024-05-03 00:45_

---

_Merged by @charliermarsh on 2024-05-03 00:51_

---

_Closed by @charliermarsh on 2024-05-03 00:51_

---

_Branch deleted on 2024-05-03 00:51_

---

_Comment by @github-actions[bot] on 2024-05-03 00:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+25 -25 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_callbacks__models.py#L24'>tests/unit/bokeh/models/test_callbacks__models.py:24:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_callbacks__models.py#L24'>tests/unit/bokeh/models/test_callbacks__models.py:24:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Absolute/Integrations/Absolute/Absolute_test.py#L7'>Packs/Absolute/Integrations/Absolute/Absolute_test.py:7:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Absolute/Integrations/Absolute/Absolute_test.py#L7'>Packs/Absolute/Integrations/Absolute/Absolute_test.py:7:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py#L3'>Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py#L3'>Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py#L3'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py#L3'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py#L8'>Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py:8:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py#L8'>Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py:8:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py#L3'>Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py#L3'>Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py#L5'>Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py:5:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py#L5'>Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py:5:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py#L5'>Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py:5:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py#L5'>Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py:5:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py#L6'>Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py:6:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py#L6'>Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py:6:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py#L7'>Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py:7:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py#L7'>Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py:7:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Inventa/Integrations/Inventa/Inventa_test.py#L15'>Packs/Inventa/Integrations/Inventa/Inventa_test.py:15:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Inventa/Integrations/Inventa/Inventa_test.py#L15'>Packs/Inventa/Integrations/Inventa/Inventa_test.py:15:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py#L3'>Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py#L3'>Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py#L3'>Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py#L3'>Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py#L8'>Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py:8:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py#L8'>Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py:8:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py#L3'>Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py#L3'>Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py#L1'>Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py:1:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py#L1'>Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py:1:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py#L2'>Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py:2:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py#L2'>Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py:2:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py#L5'>Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py:5:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py#L5'>Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py:5:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py#L2'>Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py:2:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py#L2'>Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py:2:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py#L1'>Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py:1:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py#L1'>Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py:1:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py#L3'>Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py#L3'>Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py#L3'>Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py#L3'>Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/Authentication/Authentication_test.py#L1'>Templates/Integrations/Authentication/Authentication_test.py:1:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/Authentication/Authentication_test.py#L1'>Templates/Integrations/Authentication/Authentication_test.py:1:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/CaseManagement/CaseManagement_test.py#L6'>Templates/Integrations/CaseManagement/CaseManagement_test.py:6:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/CaseManagement/CaseManagement_test.py#L6'>Templates/Integrations/CaseManagement/CaseManagement_test.py:6:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py#L7'>Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py:7:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py#L7'>Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py:7:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT013 | 50 | 25 | 25 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+25 -25 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_callbacks__models.py#L24'>tests/unit/bokeh/models/test_callbacks__models.py:24:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_callbacks__models.py#L24'>tests/unit/bokeh/models/test_callbacks__models.py:24:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Absolute/Integrations/Absolute/Absolute_test.py#L7'>Packs/Absolute/Integrations/Absolute/Absolute_test.py:7:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Absolute/Integrations/Absolute/Absolute_test.py#L7'>Packs/Absolute/Integrations/Absolute/Absolute_test.py:7:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py#L3'>Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py#L3'>Packs/ApiModules/Scripts/AWSApiModule/AWSApiModule_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py#L3'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py#L3'>Packs/BMCDiscovery/Integrations/BMCDiscovery/BMCDiscovery_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py#L8'>Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py:8:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py#L8'>Packs/CadoResponse/Integrations/CadoResponse/CadoResponse_test.py:8:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py#L3'>Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py#L3'>Packs/CrowdStrikeFalconStreamingV2/Integrations/CrowdStrikeFalconStreamingV2/CrowdStrikeFalconStreamingV2_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py#L5'>Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py:5:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py#L5'>Packs/DeprecatedContent/Integrations/PaloAltoNetworksCortex/PaloAltoNetworksCortex_test.py:5:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py#L5'>Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py:5:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py#L5'>Packs/GreyNoise/Integrations/GreyNoise/GreyNoise_test.py:5:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py#L6'>Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py:6:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py#L6'>Packs/GuardiCore/Integrations/GuardiCoreV2/GuardiCoreV2_test.py:6:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py#L7'>Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py:7:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py#L7'>Packs/HPEArubaClearPass/Integrations/HPEArubaClearPass/HPEArubaClearPass_test.py:7:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Inventa/Integrations/Inventa/Inventa_test.py#L15'>Packs/Inventa/Integrations/Inventa/Inventa_test.py:15:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Inventa/Integrations/Inventa/Inventa_test.py#L15'>Packs/Inventa/Integrations/Inventa/Inventa_test.py:15:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py#L3'>Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py#L3'>Packs/Lokpath_Keylight/Scripts/KeylightCreateIssue/KeylightCreateIssue_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py#L3'>Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py#L3'>Packs/MobileIronUEM/Integrations/MobileIronCORE/MobileIronCORE_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py#L8'>Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py:8:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py#L8'>Packs/PcapAnalysis/Scripts/PcapFileExtractor/PcapFileExtractor_test.py:8:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py#L3'>Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py#L3'>Packs/PhishUp/Integrations/PhishUp/PhishUp_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py#L1'>Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py:1:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py#L1'>Packs/RecordedFuture/Integrations/RecordedFutureLists/RecordedFutureLists_test.py:1:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py#L2'>Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py:2:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py#L2'>Packs/SingleConnect/Integrations/SingleConnect/SingleConnect_test.py:2:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py#L5'>Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py:5:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py#L5'>Packs/SophosCentral/Integrations/SophosCentral/SophosCentral_test.py:5:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py#L2'>Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py:2:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py#L2'>Packs/SplunkPy/Scripts/SplunkShowDrilldown/SplunkShowDrilldown_test.py:2:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py#L1'>Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py:1:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py#L1'>Packs/SymantecDLP/Integrations/SymantecDLP/SymantecDLP_test.py:1:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py#L3'>Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py#L3'>Packs/SymantecDLP/Integrations/SymantecDLPV2/SymantecDLPV2_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py#L3'>Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py:3:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py#L3'>Templates/Integrations/AnalyticsAndSIEM/AnalyticsAndSIEM_test.py:3:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/Authentication/Authentication_test.py#L1'>Templates/Integrations/Authentication/Authentication_test.py:1:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/Authentication/Authentication_test.py#L1'>Templates/Integrations/Authentication/Authentication_test.py:1:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/CaseManagement/CaseManagement_test.py#L6'>Templates/Integrations/CaseManagement/CaseManagement_test.py:6:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/CaseManagement/CaseManagement_test.py#L6'>Templates/Integrations/CaseManagement/CaseManagement_test.py:6:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
- <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py#L7'>Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py:7:1:</a> PT013 Found incorrect import of pytest, use simple `import pytest` instead
+ <a href='https://github.com/demisto/content/blob/66ab59b78277ee6ccdfbbee78816ade762d946fa/Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py#L7'>Templates/Integrations/DataEnrichmentThreatIntelligence/DataEnrichmentThreatIntelligence_test.py:7:1:</a> PT013 Incorrect import of `pytest`; use `import pytest` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT013 | 50 | 25 | 25 | 0 | 0 |

</p>
</details>




---
