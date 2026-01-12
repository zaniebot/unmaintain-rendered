```yaml
number: 12021
title: "Redirect `PLR1701` to `SIM101`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: dhruv/redirect-PLR1701-to-SIM101
created_at: 2024-06-25T05:50:55Z
updated_at: 2024-06-25T13:51:35Z
url: https://github.com/astral-sh/ruff/pull/12021
synced_at: 2026-01-12T15:55:40Z
```

# Redirect `PLR1701` to `SIM101`

---

_@dhruvmanila_

## Summary

This rule removes `PLR1701` and redirects it to `SIM101`.

In addition to that, the `SIM101` autofix has been fixed to add padding if required.

### `PLR1701` has bugs

It also seems that the implementation of `PLR1701` is incorrect in multiple scenarios. For example, the following code snippet:
```py
# There are two _different_ variables `a` and `b`
if isinstance(a, int) or isinstance(b, bool) or isinstance(a, float):
    pass
# There's another condition `or 1`
if isinstance(self.k, int) or isinstance(self.k, float) or 1:
    pass
```
is fixed to:
```py
# Fixed to only considering variable `a`
if isinstance(a, (float, int)):
    pass
# The additional condition is not present in the fix
if isinstance(self.k, (float, int)):
    pass
```

Playground: https://play.ruff.rs/6cfbdfb7-f183-43b0-b59e-31e728b34190

## Documentation Preview

### `PLR1701`

<img width="1397" alt="Screenshot 2024-06-25 at 11 14 40" src="https://github.com/astral-sh/ruff/assets/67177269/779ee84d-7c4d-4bb8-a3a4-c2b23a313eba">

## Test Plan

Remove the test cases for `PLR1701`, port the padding test case to `SIM101` and update the snapshot.


---

_Label `rule` added by @dhruvmanila on 2024-06-25 05:51_

---

_Comment by @github-actions[bot] on 2024-06-25 06:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -11 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ec09600d188a09fb7a90578781135fd670b075d8/airflow/providers/microsoft/azure/hooks/msgraph.py#L327'>airflow/providers/microsoft/azure/hooks/msgraph.py:327:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(data, (BytesIO, bytes, str))`
- <a href='https://github.com/apache/airflow/blob/ec09600d188a09fb7a90578781135fd670b075d8/airflow/serialization/pydantic/dag.py#L45'>airflow/serialization/pydantic/dag.py:45:9:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/apache/airflow/blob/ec09600d188a09fb7a90578781135fd670b075d8/airflow/serialization/pydantic/taskinstance.py#L70'>airflow/serialization/pydantic/taskinstance.py:70:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, (BaseOperator, MappedOperator))`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 Merge `isinstance` calls: `isinstance(x, bool | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, bool | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, list | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, float | int | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(value, bool | str)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1701 | 11 | 0 | 11 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -11 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ec09600d188a09fb7a90578781135fd670b075d8/airflow/providers/microsoft/azure/hooks/msgraph.py#L327'>airflow/providers/microsoft/azure/hooks/msgraph.py:327:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(data, (BytesIO, bytes, str))`
- <a href='https://github.com/apache/airflow/blob/ec09600d188a09fb7a90578781135fd670b075d8/airflow/serialization/pydantic/dag.py#L45'>airflow/serialization/pydantic/dag.py:45:9:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/apache/airflow/blob/ec09600d188a09fb7a90578781135fd670b075d8/airflow/serialization/pydantic/taskinstance.py#L70'>airflow/serialization/pydantic/taskinstance.py:70:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, (BaseOperator, MappedOperator))`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 Merge `isinstance` calls: `isinstance(x, bool | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, bool | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, list | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, float | int | str)`
- <a href='https://github.com/demisto/content/blob/81e706f6a35bd9cadd78746dd2045d7f92262096/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(value, bool | str)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1701 | 11 | 0 | 11 | 0 | 0 |

</p>
</details>




---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-06-25 06:17_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-25 08:36_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-25 09:36_

---

_@charliermarsh approved on 2024-06-25 10:54_

---

_Merged by @dhruvmanila on 2024-06-25 13:51_

---

_Closed by @dhruvmanila on 2024-06-25 13:51_

---

_Branch deleted on 2024-06-25 13:51_

---
