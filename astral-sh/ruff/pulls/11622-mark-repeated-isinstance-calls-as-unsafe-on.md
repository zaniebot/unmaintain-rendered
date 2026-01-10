```yaml
number: 11622
title: "Mark `repeated-isinstance-calls` as unsafe on Python 3.10 and later"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2024-05-30T18:01:52Z
updated_at: 2024-05-30T18:14:54Z
url: https://github.com/astral-sh/ruff/pull/11622
synced_at: 2026-01-10T21:56:00Z
```

# Mark `repeated-isinstance-calls` as unsafe on Python 3.10 and later

---

_Pull request opened by @charliermarsh on 2024-05-30 18:01_

## Summary

Closes https://github.com/astral-sh/ruff/issues/11616.

---

_Label `fixes` added by @charliermarsh on 2024-05-30 18:01_

---

_Merged by @charliermarsh on 2024-05-30 18:05_

---

_Closed by @charliermarsh on 2024-05-30 18:05_

---

_Branch deleted on 2024-05-30 18:05_

---

_Comment by @github-actions[bot] on 2024-05-30 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -18 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +0 -18 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(val, dict | list)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(obj, date | datetime)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 Merge `isinstance` calls: `isinstance(x, bool | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, bool | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, bool | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(v, bool | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, list | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(obj, list | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 [*] Merge `isinstance` calls
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py#L13'>Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py:13:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(root[i], int | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py#L13'>Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py:13:16:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(root[i], int | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, float | int | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(v, float | int | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(value, bool | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(value, bool | str)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1701 | 18 | 0 | 0 | 0 | 18 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -18 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +0 -18 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(val, dict | list)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(obj, date | datetime)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 Merge `isinstance` calls: `isinstance(x, bool | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, bool | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, bool | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(v, bool | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, list | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(obj, list | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 [*] Merge `isinstance` calls
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py#L13'>Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py:13:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(root[i], int | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py#L13'>Packs/CofenseVision/Scripts/ConvertDictOfListToListOfDict/ConvertDictOfListToListOfDict.py:13:16:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(root[i], int | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, float | int | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(v, float | int | str)`
+ <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(value, bool | str)`
- <a href='https://github.com/demisto/content/blob/a6f810d1ce537a04b20c097aea615bd6d5332859/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(value, bool | str)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1701 | 18 | 0 | 0 | 0 | 18 |

</p>
</details>




---
