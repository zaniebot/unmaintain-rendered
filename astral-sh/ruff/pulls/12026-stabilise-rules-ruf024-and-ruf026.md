```yaml
number: 12026
title: Stabilise rules RUF024 and RUF026
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: ruff-specific-stabilisations
created_at: 2024-06-25T11:23:51Z
updated_at: 2024-06-25T15:15:23Z
url: https://github.com/astral-sh/ruff/pull/12026
synced_at: 2026-01-12T15:55:40Z
```

# Stabilise rules RUF024 and RUF026

---

_@AlexWaygood_

## Summary

Stabilise the following rules in the `RUF` category for Ruff v0.5:

- `mutable-fromkeys-value`
- ~~`unnecessary-dict-comprehension`~~
- `default-factory-kwarg`
- ~~`parenthesize-chained-operators`~~
- ~~`unsorted-dunder-all`~~
- ~~`unsorted-dunder-slots`~~

These rules have been in the preview category for several minor releases, and were released to users >90 days ago. There are no open issues about any of them, and there have not been any open issues about any of them for several months. I skimmed over the implementations for all of them, and they all look reasonable to me (except for one test fixture for `RUF026` that wasn't testing what it was meant to be testing -- I fix that here as part of this PR).

## Test Plan

`cargo test`

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-25 11:23_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-06-25 11:23_

---

_Label `rule` added by @MichaReiser on 2024-06-25 11:28_

---

_Comment by @charliermarsh on 2024-06-25 11:50_

Generally looks reasonable (though we should review the ecosystem results when we can). `ParenthesizeChainedOperators` is the only one I'm unsure of because it's more opinionated but let's see what the ecosystem says.

---

_Comment by @MichaReiser on 2024-06-25 12:49_

> `unsorted-dunder-all`, `unsorted-dunder-slots` 

could also be considered very-opinionated, but I'm not sure if this should prevent us from stabilizing rules (I don't think we should implement new once until we figured out rule categorization).

---

_Comment by @github-actions[bot] on 2024-06-25 13:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -11 violations, +0 -0 fixes in 3 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c310159bc2363c12110b11febd5febaab8670210/airflow/providers/microsoft/azure/hooks/msgraph.py#L327'>airflow/providers/microsoft/azure/hooks/msgraph.py:327:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(data, (BytesIO, bytes, str))`
- <a href='https://github.com/apache/airflow/blob/c310159bc2363c12110b11febd5febaab8670210/airflow/serialization/pydantic/dag.py#L45'>airflow/serialization/pydantic/dag.py:45:9:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/apache/airflow/blob/c310159bc2363c12110b11febd5febaab8670210/airflow/serialization/pydantic/taskinstance.py#L70'>airflow/serialization/pydantic/taskinstance.py:70:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, (BaseOperator, MappedOperator))`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 Merge `isinstance` calls: `isinstance(x, bool | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, bool | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, list | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, float | int | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(value, bool | str)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/53bf1889993696d4471cdaeffce9db1646b89ffa/testing/test_runner.py#L536'>testing/test_runner.py:536:39:</a> RUF024 Do not pass mutable objects as values to `dict.fromkeys`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1701 | 11 | 0 | 11 | 0 | 0 |
| RUF024 | 1 | 1 | 0 | 0 | 0 |

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
- <a href='https://github.com/apache/airflow/blob/c310159bc2363c12110b11febd5febaab8670210/airflow/providers/microsoft/azure/hooks/msgraph.py#L327'>airflow/providers/microsoft/azure/hooks/msgraph.py:327:12:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(data, (BytesIO, bytes, str))`
- <a href='https://github.com/apache/airflow/blob/c310159bc2363c12110b11febd5febaab8670210/airflow/serialization/pydantic/dag.py#L45'>airflow/serialization/pydantic/dag.py:45:9:</a> PLR1701 [*] Merge `isinstance` calls
- <a href='https://github.com/apache/airflow/blob/c310159bc2363c12110b11febd5febaab8670210/airflow/serialization/pydantic/taskinstance.py#L70'>airflow/serialization/pydantic/taskinstance.py:70:8:</a> PLR1701 [*] Merge `isinstance` calls: `isinstance(x, (BaseOperator, MappedOperator))`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py#L196'>Packs/ANYRUN/Integrations/ANYRUN/ANYRUN.py:196:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(val, dict | list)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py#L33'>Packs/AWS-GuardDuty/Integrations/AWSGuardDutyEventCollector/AWSGuardDutyEventCollector.py:33:12:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, date | datetime)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L187'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:187:23:</a> PLR1701 Merge `isinstance` calls: `isinstance(x, bool | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py#L192'>Packs/Base/Scripts/DBotMLFetchData/DBotMLFetchData.py:192:37:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, bool | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L422'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:422:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(obj, list | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py#L256'>Packs/BluecatAddressManager/Integrations/BluecatAddressManager/BluecatAddressManager.py:256:12:</a> PLR1701 Merge `isinstance` calls
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py#L15'>Packs/ExpanseV2/Scripts/ExpanseEvidenceDynamicSection/ExpanseEvidenceDynamicSection.py:15:8:</a> PLR1701 Merge `isinstance` calls: `isinstance(v, float | int | str)`
- <a href='https://github.com/demisto/content/blob/e08a0b5fecc83a91a0ed07de6f7887553d43f98f/Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py#L577'>Packs/TOPdesk/Integrations/TOPdesk/TOPdesk.py:577:16:</a> PLR1701 Merge `isinstance` calls: `isinstance(value, bool | str)`
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

_Renamed from "Stabilise rules RUF021-RUF026" to "Stabilise rules RUF024-RUF026" by @AlexWaygood on 2024-06-25 13:34_

---

_Comment by @AlexWaygood on 2024-06-25 13:38_

For now, I've struck the following rules (all authored by me 😭) off the list:

- RUF021 (`parenthesize-chained-operators`)
- RUF022 (`unsorted-dunder-all`)
- RUF023 (`unsorted-dunder-slots`)

These rules are all working as designed, but they're all quite opinionated. Right now, it seems like promoting them to stable would be too disruptive for users who select the entire `RUF` category in their configuration file. Long-term, we'll need to figure out how to stabilise opinionated rules while minimising disruption for our users (which relates to our long-term ambition to rework our rule categorisation)

---

_@MichaReiser approved on 2024-06-25 13:39_

---

_Comment by @AlexWaygood on 2024-06-25 15:03_

> For now, I've struck the following rules (all authored by me 😭) off the list:

And I've now struck off RUF025 for similar reasons. Somewhat opinionated; has a fair few ecosystem hits; we can maybe consider it separately, but not for this PR...

---

_Renamed from "Stabilise rules RUF024-RUF026" to "Stabilise rules RUF024 and RUF026" by @AlexWaygood on 2024-06-25 15:03_

---

_Merged by @AlexWaygood on 2024-06-25 15:04_

---

_Closed by @AlexWaygood on 2024-06-25 15:04_

---

_Branch deleted on 2024-06-25 15:04_

---
