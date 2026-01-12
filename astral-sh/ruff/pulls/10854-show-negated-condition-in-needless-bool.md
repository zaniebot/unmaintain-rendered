```yaml
number: 10854
title: "Show negated condition in `needless-bool` diagnostics"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/negate
created_at: 2024-04-10T04:02:14Z
updated_at: 2024-04-10T04:35:55Z
url: https://github.com/astral-sh/ruff/pull/10854
synced_at: 2026-01-12T15:55:33Z
```

# Show negated condition in `needless-bool` diagnostics

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/10843.


---

_Label `bug` added by @charliermarsh on 2024-04-10 04:02_

---

_Comment by @github-actions[bot] on 2024-04-10 04:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `j in ["Description", "Identifier"]` directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `not j in ["Description", "Identifier"]` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `not (value.lower() == "false" or value == "0")` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `value.lower() == "false" or value == "0"` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get("statusCode") == 201` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get('statusCode') == 201` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == '0'` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `not binary == "0"` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zproject/backends.py#L628'>zproject/backends.py:628:5:</a> SIM103 Return the condition `bool(find_ldap_users_by_email(email))` directly
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zproject/backends.py#L628'>zproject/backends.py:628:5:</a> SIM103 Return the condition `find_ldap_users_by_email(email)` directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+97 -97 violations, +0 -0 fixes in 10 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/configuration.py#L1309'>airflow/configuration.py:1309:13:</a> SIM103 Return the condition `not value is None` directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/configuration.py#L1309'>airflow/configuration.py:1309:13:</a> SIM103 Return the condition `value is None` directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/dag_processing/manager.py#L1248'>airflow/dag_processing/manager.py:1248:9:</a> SIM103 Return the condition `not self._num_run < self._max_runs` directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/dag_processing/manager.py#L1248'>airflow/dag_processing/manager.py:1248:9:</a> SIM103 Return the condition `self._num_run < self._max_runs` directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/models/taskinstance.py#L385'>airflow/models/taskinstance.py:385:5:</a> SIM103 Return the condition `isinstance(value, (bytearray, bytes, str))` directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/models/taskinstance.py#L385'>airflow/models/taskinstance.py:385:5:</a> SIM103 Return the condition `not isinstance(value, (bytearray, bytes, str))` directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/providers/amazon/aws/hooks/s3.py#L649'>airflow/providers/amazon/aws/hooks/s3.py:649:13:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/providers/amazon/aws/hooks/s3.py#L649'>airflow/providers/amazon/aws/hooks/s3.py:649:13:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/providers/amazon/aws/sensors/athena.py#L97'>airflow/providers/amazon/aws/sensors/athena.py:97:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
- <a href='https://github.com/apache/airflow/blob/0af5d923d99591576b3758ab3c694d02dbe152bf/airflow/providers/amazon/aws/sensors/athena.py#L97'>airflow/providers/amazon/aws/sensors/athena.py:97:9:</a> SIM103 Return the condition `state in self.INTERMEDIATE_STATES` directly
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `username == "bokeh" and password == "bokeh"` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+25 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the condition `RDL is None` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not not self._valid(self.rcs)` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `bool(entries.get('flat'))` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `entries.get('flat')` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not 1 < n_labels < n_samples` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not not 1 < n_labels < n_samples` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `bool(res and res['status'] == DONE_STATUS)` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `res and res['status'] == DONE_STATUS` directly
+ <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `not (value.lower() == "false" or value == "0")` directly
- <a href='https://github.com/demisto/content/blob/256a29e0916020e902d42c3d04bd903d02b68e2b/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `value.lower() == "false" or value == "0"` directly
... 38 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `not pwd_flag == "NP"` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `pwd_flag == "NP"` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `bool(self.fingerprint)` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `self.fingerprint` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1425'>securedrop/pretty_bad_protocol/_parsers.py:1425:9:</a> SIM103 Return the condition `len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1425'>securedrop/pretty_bad_protocol/_parsers.py:1425:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1788'>securedrop/pretty_bad_protocol/_parsers.py:1788:9:</a> SIM103 Return the condition `bool(self.ok)` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1788'>securedrop/pretty_bad_protocol/_parsers.py:1788:9:</a> SIM103 Return the condition `self.ok` directly
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/client/check.py#L166'>pymilvus/client/check.py:166:5:</a> SIM103 Return the condition `(end_date - start_date).days < 0` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/client/check.py#L166'>pymilvus/client/check.py:166:5:</a> SIM103 Return the condition `not (end_date - start_date).days < 0` directly
- <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/client/check.py#L20'>pymilvus/client/check.py:20:5:</a> SIM103 Return the condition `not is_legal_host(a[0]) or not is_legal_port(a[1])` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/client/check.py#L20'>pymilvus/client/check.py:20:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/client/check.py#L27'>pymilvus/client/check.py:27:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/client/check.py#L27'>pymilvus/client/check.py:27:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/orm/collection.py#L1438'>pymilvus/orm/collection.py:1438:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/orm/collection.py#L1438'>pymilvus/orm/collection.py:1438:9:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/orm/iterator.py#L458'>pymilvus/orm/iterator.py:458:9:</a> SIM103 Return the condition `cached_page is None or len(cached_page) < count` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/2112ee2096d3dfe9fdc34217a0d8d756d1a03670/pymilvus/orm/iterator.py#L458'>pymilvus/orm/iterator.py:458:9:</a> SIM103 Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not len(c.__abstractmethods__)` directly
+ <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not not len(c.__abstractmethods__)` directly
- <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `_tracking_uri or MLFLOW_TRACKING_URI.get()` directly
+ <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `bool(_tracking_uri or MLFLOW_TRACKING_URI.get())` directly
- <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/transformers/__init__.py#L1465'>mlflow/transformers/__init__.py:1465:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/transformers/__init__.py#L1465'>mlflow/transformers/__init__.py:1465:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/682796c1063fb074ac6d352da1996ecc21b1840c/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the negated condition directly
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/cibuildwheel/blob/9cf99e78bc06d33fb2947de5820be96ad9c7152c/cibuildwheel/projectfiles.py#L42'>cibuildwheel/projectfiles.py:42:5:</a> SIM103 Return the condition `len(consts) != 1` directly
+ <a href='https://github.com/pypa/cibuildwheel/blob/9cf99e78bc06d33fb2947de5820be96ad9c7152c/cibuildwheel/projectfiles.py#L42'>cibuildwheel/projectfiles.py:42:5:</a> SIM103 Return the condition `not len(consts) != 1` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/9073a2781b88d36cfb830a43c5505ec8e0081d62/reflex/utils/telemetry.py#L85'>reflex/utils/telemetry.py:85:5:</a> SIM103 Return the condition `not should_skip_compile()` directly
- <a href='https://github.com/reflex-dev/reflex/blob/9073a2781b88d36cfb830a43c5505ec8e0081d62/reflex/utils/telemetry.py#L85'>reflex/utils/telemetry.py:85:5:</a> SIM103 Return the condition `should_skip_compile()` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/skbuild/setuptools_wrap.py#L348'>skbuild/setuptools_wrap.py:348:5:</a> SIM103 Return the condition `"sdist" in given_commands and cmake_with_sdist` directly
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/skbuild/setuptools_wrap.py#L348'>skbuild/setuptools_wrap.py:348:5:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/scripts/lib/zulip_tools.py#L546'>scripts/lib/zulip_tools.py:546:5:</a> SIM103 Return the condition `"posix" in os.name and os.geteuid() == 0` directly
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/scripts/lib/zulip_tools.py#L546'>scripts/lib/zulip_tools.py:546:5:</a> SIM103 Return the condition `bool("posix" in os.name and os.geteuid() == 0)` directly
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/decorator.py#L1025'>zerver/decorator.py:1025:9:</a> SIM103 Return the condition `not user_has_device(user)` directly
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/decorator.py#L1025'>zerver/decorator.py:1025:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/avatar.py#L144'>zerver/lib/avatar.py:144:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/avatar.py#L144'>zerver/lib/avatar.py:144:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/compatibility.py#L157'>zerver/lib/compatibility.py:157:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/compatibility.py#L157'>zerver/lib/compatibility.py:157:5:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/message.py#L1425'>zerver/lib/message.py:1425:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/message.py#L1425'>zerver/lib/message.py:1425:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/message.py#L1439'>zerver/lib/message.py:1439:5:</a> SIM103 Return the condition `not user_profile.realm != message.get_realm()` directly
- <a href='https://github.com/zulip/zulip/blob/15cec69995d6700b2b0ec895f31abd03d2037e83/zerver/lib/message.py#L1439'>zerver/lib/message.py:1439:5:</a> SIM103 Return the condition `user_profile.realm != message.get_realm()` directly
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 194 | 97 | 97 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-10 04:29_

---

_Closed by @charliermarsh on 2024-04-10 04:29_

---

_Branch deleted on 2024-04-10 04:29_

---
