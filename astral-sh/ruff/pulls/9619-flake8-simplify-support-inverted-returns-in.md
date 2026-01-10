```yaml
number: 9619
title: "[`flake8-simplify`] Support inverted returns in `needless-bool` (`SIM103`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/not
created_at: 2024-01-23T03:21:15Z
updated_at: 2024-01-23T03:34:32Z
url: https://github.com/astral-sh/ruff/pull/9619
synced_at: 2026-01-10T22:57:09Z
```

# [`flake8-simplify`] Support inverted returns in `needless-bool` (`SIM103`)

---

_Pull request opened by @charliermarsh on 2024-01-23 03:21_

Closes https://github.com/astral-sh/ruff/issues/9618.

---

_Label `bug` added by @charliermarsh on 2024-01-23 03:21_

---

_Merged by @charliermarsh on 2024-01-23 03:26_

---

_Closed by @charliermarsh on 2024-01-23 03:26_

---

_Branch deleted on 2024-01-23 03:26_

---

_Comment by @github-actions[bot] on 2024-01-23 03:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition `type_ == "table" and (name.startswith("celery_") or name == "session")` directly
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition `res['Type'] == entryTypes['error'] and 'Item not found' in res['Contents']` directly
+ <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get("statusCode") == 201` directly
- <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get('statusCode') == 201` directly
- <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == "0"` directly
+ <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == '0'` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/fad3510767c3098902eb6b345e5816cfa6de6b05/zerver/models/users.py#L627'>zerver/models/users.py:627:9:</a> SIM103 Return the condition `self.is_realm_admin and self.realm == target_user.realm` directly
+ <a href='https://github.com/zulip/zulip/blob/fad3510767c3098902eb6b345e5816cfa6de6b05/zerver/models/users.py#L627'>zerver/models/users.py:627:9:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition `type_ == "table" and (name.startswith("celery_") or name == "session")` directly
+ <a href='https://github.com/apache/airflow/blob/1c958a2ffa52bb13f82452c66a3f94cc6cd0aa44/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition `res['Type'] == entryTypes['error'] and 'Item not found' in res['Contents']` directly
+ <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get("statusCode") == 201` directly
- <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get('statusCode') == 201` directly
- <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == "0"` directly
+ <a href='https://github.com/demisto/content/blob/f5287ddc1dfdc73a380f265e96d3574d5d48f665/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == '0'` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/fad3510767c3098902eb6b345e5816cfa6de6b05/zerver/models/users.py#L627'>zerver/models/users.py:627:9:</a> SIM103 Return the condition `self.is_realm_admin and self.realm == target_user.realm` directly
+ <a href='https://github.com/zulip/zulip/blob/fad3510767c3098902eb6b345e5816cfa6de6b05/zerver/models/users.py#L627'>zerver/models/users.py:627:9:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>




---
