```yaml
number: 12048
title: "[`flake8-simplify`] Stabilize implicit-`else` simplifications in `needless-bool` (`SIM103`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: charlie/sim103
created_at: 2024-06-26T15:21:20Z
updated_at: 2024-06-26T15:50:12Z
url: https://github.com/astral-sh/ruff/pull/12048
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-simplify`] Stabilize implicit-`else` simplifications in `needless-bool` (`SIM103`)

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/ruff/pull/10414.

This is a good and intuitive change; we just put it in preview because it expanded scope a bit.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-06-26 15:21_

---

_Label `rule` added by @charliermarsh on 2024-06-26 15:24_

---

_@AlexWaygood approved on 2024-06-26 15:24_

for sure!

---

_Merged by @charliermarsh on 2024-06-26 15:24_

---

_Closed by @charliermarsh on 2024-06-26 15:24_

---

_Branch deleted on 2024-06-26 15:24_

---

_Comment by @github-actions[bot] on 2024-06-26 15:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+157 -0 violations, +0 -0 fixes in 12 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/configuration.py#L1315'>airflow/configuration.py:1315:13:</a> SIM103 Return the condition `not value is None` directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/dag_processing/manager.py#L1268'>airflow/dag_processing/manager.py:1268:9:</a> SIM103 Return the condition `not self._num_run < self._max_runs` directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/models/taskinstance.py#L671'>airflow/models/taskinstance.py:671:5:</a> SIM103 Return the condition `not isinstance(value, (bytearray, bytes, str))` directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/operators/python.py#L75'>airflow/operators/python.py:75:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/airbyte/triggers/airbyte.py#L120'>airflow/providers/airbyte/triggers/airbyte.py:120:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/amazon/aws/hooks/s3.py#L649'>airflow/providers/amazon/aws/hooks/s3.py:649:13:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/amazon/aws/sensors/athena.py#L97'>airflow/providers/amazon/aws/sensors/athena.py:97:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/amazon/aws/sensors/emr.py#L337'>airflow/providers/amazon/aws/sensors/emr.py:337:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/amazon/aws/sensors/opensearch_serverless.py#L112'>airflow/providers/amazon/aws/sensors/opensearch_serverless.py:112:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/dbt/cloud/triggers/dbt.py#L114'>airflow/providers/dbt/cloud/triggers/dbt.py:114:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/google/cloud/hooks/cloud_sql.py#L946'>airflow/providers/google/cloud/hooks/cloud_sql.py:946:9:</a> SIM103 Return the condition `not (val == "False" or val is False)` directly
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+42 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `bool(entries.get('flat'))` directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `1 < n_labels < n_samples` directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L28'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:28:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L34'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:34:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L40'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:40:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L46'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:46:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L52'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:52:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L58'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:58:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `bool(res and res['status'] == DONE_STATUS)` directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py#L10'>Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py:10:5:</a> SIM103 Return the condition `command1.args_lst == command2.args_lst` directly
+ <a href='https://github.com/demisto/content/blob/63c88b17db7bc6d1fbabc9750051780c648cdc24/Packs/CommonScripts/Scripts/DomainReputation/DomainReputation.py#L22'>Packs/CommonScripts/Scripts/DomainReputation/DomainReputation.py:22:5:</a> SIM103 Return the condition directly
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/admin/bootstrap.py#L120'>admin/bootstrap.py:120:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `not pwd_flag == "NP"` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/models.py#L261'>securedrop/models.py:261:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `bool(self.fingerprint)` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1425'>securedrop/pretty_bad_protocol/_parsers.py:1425:9:</a> SIM103 Return the condition `not len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1788'>securedrop/pretty_bad_protocol/_parsers.py:1788:9:</a> SIM103 Return the condition `bool(self.ok)` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L234'>securedrop/pretty_bad_protocol/_parsers.py:234:5:</a> SIM103 Return the condition `bool(HEXADECIMAL.match(string))` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/check.py#L166'>pymilvus/client/check.py:166:5:</a> SIM103 Return the condition `not (end_date - start_date).days < 0` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/check.py#L20'>pymilvus/client/check.py:20:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/check.py#L27'>pymilvus/client/check.py:27:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/orm/collection.py#L1441'>pymilvus/orm/collection.py:1441:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/orm/iterator.py#L458'>pymilvus/orm/iterator.py:458:9:</a> SIM103 Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/langchain/utils/chat.py#L246'>mlflow/langchain/utils/chat.py:246:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/sklearn/utils.py#L929'>mlflow/sklearn/utils.py:929:9:</a> SIM103 Return the condition `len(c.__abstractmethods__)` directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/tracking/_tracking_service/utils.py#L27'>mlflow/tracking/_tracking_service/utils.py:27:5:</a> SIM103 Return the condition `bool(_tracking_uri or MLFLOW_TRACKING_URI.get())` directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/transformers/__init__.py#L1459'>mlflow/transformers/__init__.py:1459:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/utils/autologging_utils/__init__.py#L275'>mlflow/utils/autologging_utils/__init__.py:275:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/utils/search_utils.py#L1242'>mlflow/utils/search_utils.py:1242:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/utils/search_utils.py#L1439'>mlflow/utils/search_utils.py:1439:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/utils/search_utils.py#L1737'>mlflow/utils/search_utils.py:1737:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/7272a358787b86c0377802ffa7dc4a793291f290/mlflow/utils/search_utils.py#L472'>mlflow/utils/search_utils.py:472:9:</a> SIM103 Return the negated condition directly
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/2552303ff3fecb3c61c70ef7a9b70e349dc62952/cibuildwheel/projectfiles.py#L42'>cibuildwheel/projectfiles.py:42:5:</a> SIM103 Return the condition `not len(consts) != 1` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/9d71bcbbb5f183a8ec06a7dce780306e94f76f33/reflex/utils/telemetry.py#L96'>reflex/utils/telemetry.py:96:5:</a> SIM103 Return the condition `not should_skip_compile()` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/ccb1b05cc7a537635cd4f177e5c4051f0e50d51a/skbuild/setuptools_wrap.py#L322'>skbuild/setuptools_wrap.py:322:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/scikit-build/scikit-build/blob/ccb1b05cc7a537635cd4f177e5c4051f0e50d51a/skbuild/setuptools_wrap.py#L347'>skbuild/setuptools_wrap.py:347:5:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+45 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/corporate/lib/stripe.py#L1041'>corporate/lib/stripe.py:1041:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/corporate/lib/stripe.py#L4015'>corporate/lib/stripe.py:4015:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/corporate/lib/stripe.py#L4425'>corporate/lib/stripe.py:4425:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/corporate/lib/stripe.py#L4873'>corporate/lib/stripe.py:4873:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/scripts/lib/zulip_tools.py#L152'>scripts/lib/zulip_tools.py:152:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/scripts/lib/zulip_tools.py#L546'>scripts/lib/zulip_tools.py:546:5:</a> SIM103 Return the condition `bool("posix" in os.name and os.geteuid() == 0)` directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/actions/user_groups.py#L128'>zerver/actions/user_groups.py:128:9:</a> SIM103 Return the condition `diff < realm.waiting_period_threshold` directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/decorator.py#L1025'>zerver/decorator.py:1025:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/avatar.py#L126'>zerver/lib/avatar.py:126:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L157'>zerver/lib/compatibility.py:157:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L40'>zerver/lib/compatibility.py:40:5:</a> SIM103 Return the condition `timezone_now() > deadline` directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/markdown/__init__.py#L2070'>zerver/lib/markdown/__init__.py:2070:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/markdown/__init__.py#L2075'>zerver/lib/markdown/__init__.py:2075:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/message.py#L1416'>zerver/lib/message.py:1416:5:</a> SIM103 Return the negated condition directly
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/80eec9687fcb0b39007013ba6a376b50091f7981/src/trio/_tools/gen_exports.py#L61'>src/trio/_tools/gen_exports.py:61:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/python-trio/trio/blob/80eec9687fcb0b39007013ba6a376b50091f7981/src/trio/_util.py#L130'>src/trio/_util.py:130:9:</a> SIM103 Return the condition `value.__class__.__name__ in ("Future", "Deferred")` directly
+ <a href='https://github.com/python-trio/trio/blob/80eec9687fcb0b39007013ba6a376b50091f7981/src/trio/testing/_raises_group.py#L236'>src/trio/testing/_raises_group.py:236:9:</a> SIM103 Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/cf82e6da59346b6f2d5c0476760a2dc77261aced/nox/virtualenv.py#L452'>nox/virtualenv.py:452:9:</a> SIM103 Return the condition directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 157 | 157 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_run.py#L113'>tests/console/commands/test_run.py:113:5:</a> F841 Local variable `tester` is assigned to but never used
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_run.py#L115'>tests/console/commands/test_run.py:115:5:</a> F841 Local variable `cmd_script_file` is assigned to but never used
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F841 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-06-26 15:50_

\cc @AlexWaygood - just flagging that there are a lot of ecosystem changes here.

---
