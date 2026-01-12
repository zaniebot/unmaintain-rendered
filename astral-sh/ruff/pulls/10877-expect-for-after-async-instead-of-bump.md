```yaml
number: 10877
title: "Expect `for` after `async` instead of `bump`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
  - fuzzer
assignees: []
merged: true
base: dhruv/parser
head: dhruv/async-for-panic
created_at: 2024-04-11T12:44:37Z
updated_at: 2024-04-19T10:41:36Z
url: https://github.com/astral-sh/ruff/pull/10877
synced_at: 2026-01-12T15:55:33Z
```

# Expect `for` after `async` instead of `bump`

---

_@dhruvmanila_

_No description provided._

---

_Label `bug` added by @dhruvmanila on 2024-04-11 12:44_

---

_Label `parser` added by @dhruvmanila on 2024-04-11 12:44_

---

_Label `fuzzer` added by @dhruvmanila on 2024-04-11 12:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-11 12:44_

---

_@dhruvmanila reviewed on 2024-04-11 12:45_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1941 on 2024-04-11 12:45_

`(async` still panics which I don't think is related.

---

_Comment by @codspeed-hq[bot] on 2024-04-11 12:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/async-for-panic)

### Merging #10877 will **degrade performances by 5.27%**

<sub>Comparing <code>dhruv/async-for-panic</code> (3b54fc5) with <code>dhruv/parser</code> (b069358)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/async-for-panic)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/async-for-panic` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.8 ms | 26.7 ms | +8.16% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.77% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.27% |


---

_Comment by @github-actions[bot] on 2024-04-11 13:05_

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
+ <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `j in ["Description", "Identifier"]` directly
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `not j in ["Description", "Identifier"]` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `not (value.lower() == "false" or value == "0")` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `value.lower() == "false" or value == "0"` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get("statusCode") == 201` directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get('statusCode') == 201` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == '0'` directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `not binary == "0"` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/b6473deca0ea7eaba66ef99cb6c1538ee444a31c/zproject/backends.py#L628'>zproject/backends.py:628:5:</a> SIM103 Return the condition `bool(find_ldap_users_by_email(email))` directly
+ <a href='https://github.com/zulip/zulip/blob/b6473deca0ea7eaba66ef99cb6c1538ee444a31c/zproject/backends.py#L628'>zproject/backends.py:628:5:</a> SIM103 Return the condition `find_ldap_users_by_email(email)` directly
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
ℹ️ ecosystem check **detected linter changes**. (+97 -237 violations, +0 -0 fixes in 12 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+24 -62 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/cli/commands/kubernetes_command.py#L70'>airflow/cli/commands/kubernetes_command.py:70:14:</a> FURB103 `open` and `write` should be replaced by `Path(...).write_text(yaml.dump(sanitized_pod))`
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/cli/commands/pool_command.py#L147'>airflow/cli/commands/pool_command.py:147:10:</a> FURB103 `open` and `write` should be replaced by `Path(filepath)....`
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/configuration.py#L1309'>airflow/configuration.py:1309:13:</a> SIM103 Return the condition `not value is None` directly
+ <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/configuration.py#L1309'>airflow/configuration.py:1309:13:</a> SIM103 Return the condition `value is None` directly
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/dag_processing/manager.py#L1248'>airflow/dag_processing/manager.py:1248:9:</a> SIM103 Return the condition `not self._num_run < self._max_runs` directly
+ <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/dag_processing/manager.py#L1248'>airflow/dag_processing/manager.py:1248:9:</a> SIM103 Return the condition `self._num_run < self._max_runs` directly
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/example_dags/example_kubernetes_executor.py#L91'>airflow/example_dags/example_kubernetes_executor.py:91:18:</a> FURB103 `open` and `write` should be replaced by `Path("/foo/volume_mount_test.txt").write_text("Hello")`
+ <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/models/taskinstance.py#L385'>airflow/models/taskinstance.py:385:5:</a> SIM103 Return the condition `isinstance(value, (bytearray, bytes, str))` directly
... 42 additional changes omitted for rule SIM103
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/providers/databricks/operators/databricks_sql.py#L152'>airflow/providers/databricks/operators/databricks_sql.py:152:18:</a> FURB103 `open` and `write` should be replaced by `Path(self._output_path)....`
- <a href='https://github.com/apache/airflow/blob/3af00fa2a206e31a449648e33eed1f806d8b62b7/airflow/providers/fab/auth_manager/security_manager/override.py#L776'>airflow/providers/fab/auth_manager/security_manager/override.py:776:18:</a> FURB103 `open` and `write` should be replaced by `Path(password_path).write_text(password)`
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -42 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L42'>examples/models/basic_plot.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Button widgets", doc))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Calendar 2014", doc))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/colors.py#L69'>examples/models/colors.py:69:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/custom.py#L117'>examples/models/custom.py:117:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/customjs.py#L38'>examples/models/customjs.py:38:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
... 36 additional changes omitted for rule FURB103
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `username == "bokeh" and password == "bokeh"` directly
... 35 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+25 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the condition `RDL is None` directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not not self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not self._valid(self.rcs)` directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `bool(entries.get('flat'))` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `entries.get('flat')` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not 1 < n_labels < n_samples` directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not not 1 < n_labels < n_samples` directly
- <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `bool(res and res['status'] == DONE_STATUS)` directly
+ <a href='https://github.com/demisto/content/blob/b2e70db1861c43b4fc49664218642ae7de5a3499/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `res and res['status'] == DONE_STATUS` directly
... 40 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `not pwd_flag == "NP"` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `pwd_flag == "NP"` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `bool(self.fingerprint)` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `self.fingerprint` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `len(self.fingerprints) == 0` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1425'>securedrop/pretty_bad_protocol/_parsers.py:1425:9:</a> SIM103 Return the condition `len(self.fingerprints) == 0` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1425'>securedrop/pretty_bad_protocol/_parsers.py:1425:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
- <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1788'>securedrop/pretty_bad_protocol/_parsers.py:1788:9:</a> SIM103 Return the condition `bool(self.ok)` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_parsers.py#L1788'>securedrop/pretty_bad_protocol/_parsers.py:1788:9:</a> SIM103 Return the condition `self.ok` directly
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
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/client/check.py#L166'>pymilvus/client/check.py:166:5:</a> SIM103 Return the condition `(end_date - start_date).days < 0` directly
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/client/check.py#L166'>pymilvus/client/check.py:166:5:</a> SIM103 Return the condition `not (end_date - start_date).days < 0` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/client/check.py#L20'>pymilvus/client/check.py:20:5:</a> SIM103 Return the condition `not is_legal_host(a[0]) or not is_legal_port(a[1])` directly
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/client/check.py#L20'>pymilvus/client/check.py:20:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/client/check.py#L27'>pymilvus/client/check.py:27:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/client/check.py#L27'>pymilvus/client/check.py:27:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/orm/collection.py#L1445'>pymilvus/orm/collection.py:1445:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/orm/collection.py#L1445'>pymilvus/orm/collection.py:1445:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/orm/iterator.py#L458'>pymilvus/orm/iterator.py:458:9:</a> SIM103 Return the condition `cached_page is None or len(cached_page) < count` directly
- <a href='https://github.com/milvus-io/pymilvus/blob/b6906e1790f2f1e7499a50831fcd456af34c0586/pymilvus/orm/iterator.py#L458'>pymilvus/orm/iterator.py:458:9:</a> SIM103 Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not len(c.__abstractmethods__)` directly
- <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not not len(c.__abstractmethods__)` directly
+ <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `_tracking_uri or MLFLOW_TRACKING_URI.get()` directly
- <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `bool(_tracking_uri or MLFLOW_TRACKING_URI.get())` directly
+ <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/transformers/__init__.py#L1469'>mlflow/transformers/__init__.py:1469:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/transformers/__init__.py#L1469'>mlflow/transformers/__init__.py:1469:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/mlflow/mlflow/blob/fc858460b719be7c1dfec414b5219533a3424455/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the negated condition directly
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
+ <a href='https://github.com/pypa/cibuildwheel/blob/9cf99e78bc06d33fb2947de5820be96ad9c7152c/cibuildwheel/projectfiles.py#L42'>cibuildwheel/projectfiles.py:42:5:</a> SIM103 Return the condition `len(consts) != 1` directly
- <a href='https://github.com/pypa/cibuildwheel/blob/9cf99e78bc06d33fb2947de5820be96ad9c7152c/cibuildwheel/projectfiles.py#L42'>cibuildwheel/projectfiles.py:42:5:</a> SIM103 Return the condition `not len(consts) != 1` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/9073a2781b88d36cfb830a43c5505ec8e0081d62/reflex/utils/telemetry.py#L85'>reflex/utils/telemetry.py:85:5:</a> SIM103 Return the condition `not should_skip_compile()` directly
+ <a href='https://github.com/reflex-dev/reflex/blob/9073a2781b88d36cfb830a43c5505ec8e0081d62/reflex/utils/telemetry.py#L85'>reflex/utils/telemetry.py:85:5:</a> SIM103 Return the condition `should_skip_compile()` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/package.py#L734'>package.py:734:14:</a> FURB103 `open` and `write` should be replaced by `Path(p12).write_bytes(certificate_data)`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/chain/ethereum/airdrops.py#L248'>rotkehlchen/chain/ethereum/airdrops.py:248:18:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(response.text, encoding='utf8')`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/chain/ethereum/airdrops.py#L284'>rotkehlchen/chain/ethereum/airdrops.py:284:14:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/chain/ethereum/utils.py#L224'>rotkehlchen/chain/ethereum/utils.py:224:10:</a> FURB103 `open` and `write` should be replaced by `Path(avatars_dir / f'{ens_name}.png').write_bytes(avatar)`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/db/dbhandler.py#L300'>rotkehlchen/db/dbhandler.py:300:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.user_data_dir / DBINFO_FILENAME).write_text(rlk_jsondumps(dbinfo), encoding='utf8')`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/db/dbhandler.py#L593'>rotkehlchen/db/dbhandler.py:593:18:</a> FURB103 `open` and `write` should be replaced by `Path(tempdbpath).write_bytes(unencrypted_db_data)`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/icons.py#L190'>rotkehlchen/icons.py:190:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.iconfile_path(asset)).write_bytes(response.content)`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/tests/api/test_caching.py#L42'>rotkehlchen/tests/api/test_caching.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/ETH_small.png').write_bytes(b'')`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/tests/api/test_caching.py#L45'>rotkehlchen/tests/api/test_caching.py:45:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/BTC_small.png').write_bytes(b'')`
- <a href='https://github.com/rotki/rotki/blob/e161317e2b56f4a77f1ae1850740b2f645837039/rotkehlchen/tests/api/test_caching.py#L48'>rotkehlchen/tests/api/test_caching.py:48:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/AVAX_small.png').write_bytes(b'')`
... 9 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 194 | 97 | 97 | 0 | 0 |
| FURB103 | 140 | 0 | 140 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1946 on 2024-04-11 16:25_

Nit
```suggestion
        let is_async = self.eat(TokenKind::Async);
        
        if is_async {
            // test_err comprehension_missing_for_after_async
            // (async)
            // (x async x in iter)
            self.expect(TokenKind::For);
        } else {
            self.bump(TokenKind::For);
        };

```

---

_@MichaReiser reviewed on 2024-04-11 16:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1941 on 2024-04-11 16:25_

I assume you plan to tackle this as a separate PR

---

_@MichaReiser reviewed on 2024-04-11 16:25_

---

_@MichaReiser approved on 2024-04-11 16:25_

---

_@dhruvmanila reviewed on 2024-04-11 16:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1941 on 2024-04-11 16:42_

Yes

---

_Merged by @dhruvmanila on 2024-04-11 16:44_

---

_Closed by @dhruvmanila on 2024-04-11 16:44_

---

_Branch deleted on 2024-04-11 16:44_

---

_@dhruvmanila reviewed on 2024-04-19 10:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1941 on 2024-04-19 10:41_

(btw, this was fixed indirectly)

---
