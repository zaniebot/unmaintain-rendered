```yaml
number: 10872
title: Review error enum variants and messages
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/review-error-message
created_at: 2024-04-11T09:36:06Z
updated_at: 2024-04-11T10:34:14Z
url: https://github.com/astral-sh/ruff/pull/10872
synced_at: 2026-01-10T22:37:01Z
```

# Review error enum variants and messages

---

_Pull request opened by @dhruvmanila on 2024-04-11 09:36_

## Summary

This PR makes the following changes in `ParseErrorType` enum variants:
1. Use `allow(...)` for variant names except for `Expected*`
2. Consistently uppercase the first letter in the message to match the diagnostic messages
3. Consistently use "... cannot follow ..." instead of "... follows ..."
4. Avoid using keyword value (like "del") directly and prefer to use descriptive name (like "delete"). This makes the message read better


---

_Label `internal` added by @dhruvmanila on 2024-04-11 09:36_

---

_Label `parser` added by @dhruvmanila on 2024-04-11 09:36_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-11 09:36_

---

_Comment by @github-actions[bot] on 2024-04-11 10:11_

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
+ <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `j in ["Description", "Identifier"]` directly
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `not j in ["Description", "Identifier"]` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `not (value.lower() == "false" or value == "0")` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py#L1606'>Packs/CimTrak-SystemIntegrityAssurance/Integrations/CimTrak/CimTrak.py:1606:5:</a> SIM103 Return the condition `value.lower() == "false" or value == "0"` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py#L221'>Packs/ContentManagement/Scripts/ConfigurationSetup/ConfigurationSetup.py:221:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get("statusCode") == 201` directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py#L1432'>Packs/Malwarebytes/Integrations/Malwarebytes/Malwarebytes.py:1432:13:</a> SIM103 Return the condition `data.get('statusCode') == 201` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `binary == '0'` directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py#L126'>Packs/TrendMicroDDA/Integrations/TrendMicroDDA/TrendMicroDDA.py:126:5:</a> SIM103 Return the condition `not binary == "0"` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/zproject/backends.py#L628'>zproject/backends.py:628:5:</a> SIM103 Return the condition `bool(find_ldap_users_by_email(email))` directly
+ <a href='https://github.com/zulip/zulip/blob/36aa0177bd0312c32503ca3e368f25664e724ef7/zproject/backends.py#L628'>zproject/backends.py:628:5:</a> SIM103 Return the condition `find_ldap_users_by_email(email)` directly
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
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/cli/commands/kubernetes_command.py#L70'>airflow/cli/commands/kubernetes_command.py:70:14:</a> FURB103 `open` and `write` should be replaced by `Path(...).write_text(yaml.dump(sanitized_pod))`
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/cli/commands/pool_command.py#L147'>airflow/cli/commands/pool_command.py:147:10:</a> FURB103 `open` and `write` should be replaced by `Path(filepath)....`
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/configuration.py#L1309'>airflow/configuration.py:1309:13:</a> SIM103 Return the condition `not value is None` directly
+ <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/configuration.py#L1309'>airflow/configuration.py:1309:13:</a> SIM103 Return the condition `value is None` directly
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/dag_processing/manager.py#L1248'>airflow/dag_processing/manager.py:1248:9:</a> SIM103 Return the condition `not self._num_run < self._max_runs` directly
+ <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/dag_processing/manager.py#L1248'>airflow/dag_processing/manager.py:1248:9:</a> SIM103 Return the condition `self._num_run < self._max_runs` directly
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/example_dags/example_kubernetes_executor.py#L91'>airflow/example_dags/example_kubernetes_executor.py:91:18:</a> FURB103 `open` and `write` should be replaced by `Path("/foo/volume_mount_test.txt").write_text("Hello")`
+ <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/migrations/env.py#L33'>airflow/migrations/env.py:33:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/models/taskinstance.py#L385'>airflow/models/taskinstance.py:385:5:</a> SIM103 Return the condition `isinstance(value, (bytearray, bytes, str))` directly
... 42 additional changes omitted for rule SIM103
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/providers/databricks/operators/databricks_sql.py#L152'>airflow/providers/databricks/operators/databricks_sql.py:152:18:</a> FURB103 `open` and `write` should be replaced by `Path(self._output_path)....`
- <a href='https://github.com/apache/airflow/blob/5ff26586cd3931d223e76aeb73770f062ff9e409/airflow/providers/fab/auth_manager/security_manager/override.py#L776'>airflow/providers/fab/auth_manager/security_manager/override.py:776:18:</a> FURB103 `open` and `write` should be replaced by `Path(password_path).write_text(password)`
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
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the condition `RDL is None` directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the negated condition directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not not self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not self._valid(self.rcs)` directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `bool(entries.get('flat'))` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `entries.get('flat')` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not 1 < n_labels < n_samples` directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not not 1 < n_labels < n_samples` directly
- <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `bool(res and res['status'] == DONE_STATUS)` directly
+ <a href='https://github.com/demisto/content/blob/e40c6fbe867cbd5e4593ce3bdf8039c2171fc8dc/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `res and res['status'] == DONE_STATUS` directly
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
+ <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not len(c.__abstractmethods__)` directly
- <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `not not len(c.__abstractmethods__)` directly
+ <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `_tracking_uri or MLFLOW_TRACKING_URI.get()` directly
- <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `bool(_tracking_uri or MLFLOW_TRACKING_URI.get())` directly
+ <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/transformers/__init__.py#L1473'>mlflow/transformers/__init__.py:1473:5:</a> SIM103 Return the condition directly
- <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/transformers/__init__.py#L1473'>mlflow/transformers/__init__.py:1473:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/mlflow/mlflow/blob/1b7b4afa84d52ab29e59a08291bc22626799c743/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the negated condition directly
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
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/package.py#L734'>package.py:734:14:</a> FURB103 `open` and `write` should be replaced by `Path(p12).write_bytes(certificate_data)`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/chain/ethereum/airdrops.py#L248'>rotkehlchen/chain/ethereum/airdrops.py:248:18:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(response.text, encoding='utf8')`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/chain/ethereum/airdrops.py#L284'>rotkehlchen/chain/ethereum/airdrops.py:284:14:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/chain/ethereum/utils.py#L224'>rotkehlchen/chain/ethereum/utils.py:224:10:</a> FURB103 `open` and `write` should be replaced by `Path(avatars_dir / f'{ens_name}.png').write_bytes(avatar)`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/db/dbhandler.py#L300'>rotkehlchen/db/dbhandler.py:300:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.user_data_dir / DBINFO_FILENAME).write_text(rlk_jsondumps(dbinfo), encoding='utf8')`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/db/dbhandler.py#L593'>rotkehlchen/db/dbhandler.py:593:18:</a> FURB103 `open` and `write` should be replaced by `Path(tempdbpath).write_bytes(unencrypted_db_data)`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/icons.py#L190'>rotkehlchen/icons.py:190:14:</a> FURB103 `open` and `write` should be replaced by `Path(self.iconfile_path(asset)).write_bytes(response.content)`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/tests/api/test_caching.py#L42'>rotkehlchen/tests/api/test_caching.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/ETH_small.png').write_bytes(b'')`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/tests/api/test_caching.py#L45'>rotkehlchen/tests/api/test_caching.py:45:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/BTC_small.png').write_bytes(b'')`
- <a href='https://github.com/rotki/rotki/blob/50590f8ab32b3d47f78f366e8cad19a73228216a/rotkehlchen/tests/api/test_caching.py#L48'>rotkehlchen/tests/api/test_caching.py:48:10:</a> FURB103 `open` and `write` should be replaced by `Path(f'{icons_dir}/AVAX_small.png').write_bytes(b'')`
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

_Merged by @dhruvmanila on 2024-04-11 10:15_

---

_Closed by @dhruvmanila on 2024-04-11 10:15_

---

_Branch deleted on 2024-04-11 10:15_

---

_Comment by @codspeed-hq[bot] on 2024-04-11 10:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/review-error-message)

### Merging #10872 will **degrade performances by 5.28%**

<sub>Comparing <code>dhruv/review-error-message</code> (ea0b7eb) with <code>dhruv/parser</code> (6b4e771)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/review-error-message)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/review-error-message` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.9 ms | 26.7 ms | +8.22% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.78% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.28% |


---
