```yaml
number: 11684
title: "[`flake8-simplify`] Simplify double negatives in `SIM103`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/inv
created_at: 2024-06-01T23:14:01Z
updated_at: 2024-06-01T23:30:56Z
url: https://github.com/astral-sh/ruff/pull/11684
synced_at: 2026-01-12T15:55:38Z
```

# [`flake8-simplify`] Simplify double negatives in `SIM103`

---

_@charliermarsh_

## Summary

Closes: https://github.com/astral-sh/ruff/issues/11685.



---

_Label `fixes` added by @charliermarsh on 2024-06-01 23:14_

---

_Merged by @charliermarsh on 2024-06-01 23:21_

---

_Closed by @charliermarsh on 2024-06-01 23:21_

---

_Branch deleted on 2024-06-01 23:21_

---

_Comment by @github-actions[bot] on 2024-06-01 23:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -8 violations, +0 -0 fixes in 4 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e3e450ede2aef341c2137eb1c30555e11892614f/airflow/providers/microsoft/azure/hooks/cosmos.py#L216'>airflow/providers/microsoft/azure/hooks/cosmos.py:216:9:</a> SIM103 Return the condition `existing_container` directly
- <a href='https://github.com/apache/airflow/blob/e3e450ede2aef341c2137eb1c30555e11892614f/airflow/providers/microsoft/azure/hooks/cosmos.py#L216'>airflow/providers/microsoft/azure/hooks/cosmos.py:216:9:</a> SIM103 Return the condition `not not existing_container` directly
+ <a href='https://github.com/apache/airflow/blob/e3e450ede2aef341c2137eb1c30555e11892614f/airflow/providers/microsoft/azure/hooks/cosmos.py#L264'>airflow/providers/microsoft/azure/hooks/cosmos.py:264:9:</a> SIM103 Return the condition `existing_database` directly
- <a href='https://github.com/apache/airflow/blob/e3e450ede2aef341c2137eb1c30555e11892614f/airflow/providers/microsoft/azure/hooks/cosmos.py#L264'>airflow/providers/microsoft/azure/hooks/cosmos.py:264:9:</a> SIM103 Return the condition `not not existing_database` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not not self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `1 < n_labels < n_samples` directly
- <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not not 1 < n_labels < n_samples` directly
+ <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py#L10'>Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py:10:5:</a> SIM103 Return the condition `command1.args_lst == command2.args_lst` directly
- <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py#L10'>Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py:10:5:</a> SIM103 Return the condition `not not command1.args_lst == command2.args_lst` directly
+ <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles.py#L3150'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles.py:3150:9:</a> SIM103 Return the condition `isOleFile(self.msg_file_path)` directly
- <a href='https://github.com/demisto/content/blob/7a62406b0639196733ecfc5094c9a4e7dae0b12f/Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles.py#L3150'>Packs/CommonScripts/Scripts/ParseEmailFiles/ParseEmailFiles.py:3150:9:</a> SIM103 Return the condition `not not isOleFile(self.msg_file_path)` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/315506d1c7c5de48fa396fc99f88a2cd2b718c6e/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `len(c.__abstractmethods__)` directly
- <a href='https://github.com/mlflow/mlflow/blob/315506d1c7c5de48fa396fc99f88a2cd2b718c6e/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not not len(c.__abstractmethods__)` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/58145836940fc18b4cbb85c3591f451b06d8477e/zerver/models/realms.py#L995'>zerver/models/realms.py:995:9:</a> SIM103 Return the condition `not not self.enable_spectator_access` directly
+ <a href='https://github.com/zulip/zulip/blob/58145836940fc18b4cbb85c3591f451b06d8477e/zerver/models/realms.py#L995'>zerver/models/realms.py:995:9:</a> SIM103 Return the condition `self.enable_spectator_access` directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 16 | 8 | 8 | 0 | 0 |

</p>
</details>




---
