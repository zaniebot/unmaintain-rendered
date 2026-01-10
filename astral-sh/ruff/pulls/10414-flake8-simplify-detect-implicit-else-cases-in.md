```yaml
number: 10414
title: "[`flake8-simplify`] Detect implicit `else` cases in `needless-bool` (`SIM103`)"
type: pull_request
state: merged
author: ottaviohartman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: thartman/sim103-detect-implicit-else
created_at: 2024-03-15T00:11:30Z
updated_at: 2024-03-18T02:05:57Z
url: https://github.com/astral-sh/ruff/pull/10414
synced_at: 2026-01-10T22:47:02Z
```

# [`flake8-simplify`] Detect implicit `else` cases in `needless-bool` (`SIM103`)

---

_Pull request opened by @ottaviohartman on 2024-03-15 00:11_

Fixes #10402 

## Summary

For SIM103, detect and simplify the following case:

[playground link](https://play.ruff.rs/d98570aa-b180-495b-8600-5c4c3fd02526)
```python
def main():
    if foo > 5:
        return True
    return False
```

## Test Plan

Unit tested only.

---

_@ottaviohartman reviewed on 2024-03-15 00:12_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM103_SIM103.py.snap`:199 on 2024-03-15 00:12_

This is what I'm referencing: when `bool()` is _not_ a builtin, this should have no fix available.

---

_Comment by @github-actions[bot] on 2024-03-15 00:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+156 -0 violations, +0 -0 fixes in 10 projects; 33 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+32 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/configuration.py#L1313'>airflow/configuration.py:1313:13:</a> SIM103 Return the condition `value is None` directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/dag_processing/manager.py#L1247'>airflow/dag_processing/manager.py:1247:9:</a> SIM103 Return the condition `self._num_run < self._max_runs` directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/models/operator.py#L45'>airflow/models/operator.py:45:5:</a> SIM103 Return the condition `task.get_closest_mapped_task_group() is not None` directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/models/taskinstance.py#L372'>airflow/models/taskinstance.py:372:5:</a> SIM103 Return the condition `isinstance(value, (bytearray, bytes, str))` directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/operators/python.py#L72'>airflow/operators/python.py:72:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/providers/airbyte/triggers/airbyte.py#L119'>airflow/providers/airbyte/triggers/airbyte.py:119:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/providers/amazon/aws/hooks/s3.py#L648'>airflow/providers/amazon/aws/hooks/s3.py:648:13:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/providers/amazon/aws/sensors/athena.py#L97'>airflow/providers/amazon/aws/sensors/athena.py:97:9:</a> SIM103 Return the condition `state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/providers/amazon/aws/sensors/emr.py#L331'>airflow/providers/amazon/aws/sensors/emr.py:331:9:</a> SIM103 Return the condition `state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/c32d41d94d428b8f70274a298158b97fac285045/airflow/providers/dbt/cloud/triggers/dbt.py#L113'>airflow/providers/dbt/cloud/triggers/dbt.py:113:9:</a> SIM103 Return the condition directly
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `username == "bokeh" and password == "bokeh"` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+46 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L763'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:763:9:</a> SIM103 Return the condition `RDL is None` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py#L778'>Packs/ARIAPacketIntelligence/Integrations/ARIAPacketIntelligence/ARIAPacketIntelligence.py:778:9:</a> SIM103 Return the condition `not self._valid(self.rcs)` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L347'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:347:5:</a> SIM103 Return the condition `entries.get('flat')` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py#L540'>Packs/Base/Scripts/DBotTrainClustering/DBotTrainClustering.py:540:5:</a> SIM103 Return the condition `not 1 < n_labels < n_samples` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L28'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:28:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L34'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:34:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L40'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:40:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L46'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:46:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L52'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:52:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py#L58'>Packs/BreachRx/Integrations/BreachRx/BreachRx_test.py:58:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py#L144'>Packs/CheckPhish/Integrations/CheckPhish/CheckPhish.py:144:5:</a> SIM103 Return the condition `res and res['status'] == DONE_STATUS` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py#L10'>Packs/CommonScripts/Scripts/DisableUserWrapper/DisableUserWrapper_test.py:10:5:</a> SIM103 Return the condition `not command1.args_lst == command2.args_lst` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/CommonScripts/Scripts/DomainReputation/DomainReputation.py#L22'>Packs/CommonScripts/Scripts/DomainReputation/DomainReputation.py:22:5:</a> SIM103 Return the condition `item['Contents'] and 'Offset' in item['Contents']` directly
+ <a href='https://github.com/demisto/content/blob/b6786ddf70d9422cce1c907abb6dfb06ac71c75c/Packs/CommonScripts/Scripts/FileReputation/FileReputation.py#L22'>Packs/CommonScripts/Scripts/FileReputation/FileReputation.py:22:5:</a> SIM103 Return the condition `item['Contents'] and 'Offset' in item['Contents']` directly
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/admin/bootstrap.py#L120'>admin/bootstrap.py:120:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/journalist_gui/journalist_gui/SecureDropUpdater.py#L23'>journalist_gui/journalist_gui/SecureDropUpdater.py:23:5:</a> SIM103 Return the condition `pwd_flag == "NP"` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/models.py#L257'>securedrop/models.py:257:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L1008'>securedrop/pretty_bad_protocol/_parsers.py:1008:9:</a> SIM103 Return the condition `self.fingerprint` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L1321'>securedrop/pretty_bad_protocol/_parsers.py:1321:9:</a> SIM103 Return the condition `len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L1425'>securedrop/pretty_bad_protocol/_parsers.py:1425:9:</a> SIM103 Return the condition `len(self.fingerprints) == 0` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L1788'>securedrop/pretty_bad_protocol/_parsers.py:1788:9:</a> SIM103 Return the condition `self.ok` directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/4586923a5621dd55b6d4ba21beccb39f31f1a6f5/securedrop/pretty_bad_protocol/_parsers.py#L234'>securedrop/pretty_bad_protocol/_parsers.py:234:5:</a> SIM103 Return the condition `HEXADECIMAL.match(string)` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/0fd83fbf44c25fc247f97afd2703740664d62bce/pymilvus/client/check.py#L166'>pymilvus/client/check.py:166:5:</a> SIM103 Return the condition `(end_date - start_date).days < 0` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0fd83fbf44c25fc247f97afd2703740664d62bce/pymilvus/client/check.py#L20'>pymilvus/client/check.py:20:5:</a> SIM103 Return the condition `not is_legal_host(a[0]) or not is_legal_port(a[1])` directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0fd83fbf44c25fc247f97afd2703740664d62bce/pymilvus/client/check.py#L27'>pymilvus/client/check.py:27:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0fd83fbf44c25fc247f97afd2703740664d62bce/pymilvus/orm/collection.py#L1438'>pymilvus/orm/collection.py:1438:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/0fd83fbf44c25fc247f97afd2703740664d62bce/pymilvus/orm/iterator.py#L458'>pymilvus/orm/iterator.py:458:9:</a> SIM103 Return the condition `cached_page is None or len(cached_page) < count` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/sklearn/utils.py#L928'>mlflow/sklearn/utils.py:928:9:</a> SIM103 Return the condition `not len(c.__abstractmethods__)` directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/tracking/_tracking_service/utils.py#L25'>mlflow/tracking/_tracking_service/utils.py:25:5:</a> SIM103 Return the condition `_tracking_uri or MLFLOW_TRACKING_URI.get()` directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/transformers/__init__.py#L1418'>mlflow/transformers/__init__.py:1418:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/autologging_utils/__init__.py#L254'>mlflow/utils/autologging_utils/__init__.py:254:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/logging_utils.py#L101'>mlflow/utils/logging_utils.py:101:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/search_utils.py#L1199'>mlflow/utils/search_utils.py:1199:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/search_utils.py#L1396'>mlflow/utils/search_utils.py:1396:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/search_utils.py#L439'>mlflow/utils/search_utils.py:439:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/search_utils.py#L872'>mlflow/utils/search_utils.py:872:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/mlflow/mlflow/blob/076f00b00f996d1177d3990e400e00790182f3c9/mlflow/utils/uri.py#L65'>mlflow/utils/uri.py:65:5:</a> SIM103 Return the condition directly
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/510382886de7114c5f109007d314d74f04b180a4/cibuildwheel/projectfiles.py#L42'>cibuildwheel/projectfiles.py:42:5:</a> SIM103 Return the condition `len(consts) != 1` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/8f3efbb3146b45bbf2e0f917c30196178f96def2/rotkehlchen/chain/bitcoin/bch/utils.py#L63'>rotkehlchen/chain/bitcoin/bch/utils.py:63:5:</a> SIM103 Return the condition `_polymod(_prefix_expand(prefix) + decoded) != 0` directly
+ <a href='https://github.com/rotki/rotki/blob/8f3efbb3146b45bbf2e0f917c30196178f96def2/rotkehlchen/chain/ethereum/defi/zerionsdk.py#L166'>rotkehlchen/chain/ethereum/defi/zerionsdk.py:166:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/rotki/rotki/blob/8f3efbb3146b45bbf2e0f917c30196178f96def2/rotkehlchen/premium/sync.py#L161'>rotkehlchen/premium/sync.py:161:9:</a> SIM103 Return the condition `diff < 3600 and not force_upload` directly
+ <a href='https://github.com/rotki/rotki/blob/8f3efbb3146b45bbf2e0f917c30196178f96def2/rotkehlchen/tests/fixtures/blockchain.py#L260'>rotkehlchen/tests/fixtures/blockchain.py:260:5:</a> SIM103 Return the condition `should_mock_web3` directly
+ <a href='https://github.com/rotki/rotki/blob/8f3efbb3146b45bbf2e0f917c30196178f96def2/rotkehlchen/utils/misc.py#L404'>rotkehlchen/utils/misc.py:404:5:</a> SIM103 Return the condition `version.dev is None` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/skbuild/setuptools_wrap.py#L323'>skbuild/setuptools_wrap.py:323:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/skbuild/setuptools_wrap.py#L348'>skbuild/setuptools_wrap.py:348:5:</a> SIM103 Return the condition `"sdist" in given_commands and cmake_with_sdist` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+45 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L1027'>corporate/lib/stripe.py:1027:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L3719'>corporate/lib/stripe.py:3719:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L4125'>corporate/lib/stripe.py:4125:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/corporate/lib/stripe.py#L4569'>corporate/lib/stripe.py:4569:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/scripts/lib/zulip_tools.py#L150'>scripts/lib/zulip_tools.py:150:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/scripts/lib/zulip_tools.py#L549'>scripts/lib/zulip_tools.py:549:5:</a> SIM103 Return the condition `"posix" in os.name and os.geteuid() == 0` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/actions/user_groups.py#L132'>zerver/actions/user_groups.py:132:9:</a> SIM103 Return the condition `diff < realm.waiting_period_threshold` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/decorator.py#L1025'>zerver/decorator.py:1025:9:</a> SIM103 Return the condition `not user_has_device(user)` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/lib/avatar.py#L144'>zerver/lib/avatar.py:144:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/lib/compatibility.py#L157'>zerver/lib/compatibility.py:157:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/lib/compatibility.py#L40'>zerver/lib/compatibility.py:40:5:</a> SIM103 Return the condition `timezone_now() > deadline` directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/lib/markdown/__init__.py#L2057'>zerver/lib/markdown/__init__.py:2057:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/lib/markdown/__init__.py#L2062'>zerver/lib/markdown/__init__.py:2062:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/454f2f2d95dc8b8550af3b388b4b7441fc1e32d7/zerver/lib/message.py#L1381'>zerver/lib/message.py:1381:5:</a> SIM103 Return the condition directly
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 156 | 156 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-03-15 00:33_

Thanks! Do you mind moving this to preview-only?

> Is there a preferred way to get the next statement or expression...

Yes, the best prior art here would be... `convert_for_loop_to_any_all`. It will look like:

```rust
let parent = checker.semantic().current_statement_parent()?;
let suite = traversal::suite(stmt, parent)?;
let sibling = traversal::next_sibling(stmt, suite)?;
```

So, ideally, we check if the statement is an `if` that returns a boolean constant, and _then_ grab the next statement if so.


---

_Comment by @ottaviohartman on 2024-03-15 00:49_

> Thanks! Do you mind moving this to preview-only?

Yep! Is there a way to partially move `SIM103` to preview? Or shall I create a new rule? (if so, `SIM???`)
```python
        (Flake8Simplify, "103") => (RuleGroup::Stable, rules::flake8_simplify::rules::NeedlessBool),
```

> > Is there a preferred way to get the next statement or expression...
> 
> Yes, the best prior art here would be... `convert_for_loop_to_any_all`. It will look like:
> 
> ```rust
> let parent = checker.semantic().current_statement_parent()?;
> let suite = traversal::suite(stmt, parent)?;
> let sibling = traversal::next_sibling(stmt, suite)?;
> ```
> 
> So, ideally, we check if the statement is an `if` that returns a boolean constant, and _then_ grab the next statement if so.

Ah, I'll refactor to use that instead!

---

_Comment by @charliermarsh on 2024-03-15 00:50_

> Yep! Is there a way to partially move SIM103 to preview?

So typically we'd just add a check for the new codepath on `checker.settings.preview.is_enabled()`.

---

_Comment by @codspeed-hq[bot] on 2024-03-15 01:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ottaviohartman:thartman/sim103-detect-implicit-else)

### Merging #10414 will **not alter performance**

<sub>Comparing <code>ottaviohartman:thartman/sim103-detect-implicit-else</code> (7ecc53d) with <code>main</code> (8619986)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @ottaviohartman on 2024-03-15 01:55_

Ready for another review @charliermarsh . Thanks for the help!

1. Used (much simpler) sibling logic.
1. Added `if preview` to guard this new change.
1. Created preview insta snap.

---

_Comment by @ottaviohartman on 2024-03-15 02:07_

Looks like my clone() is slowing down perf tests. I'll look into that.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:80 on 2024-03-15 02:10_

If possible... we should try to only do this if we know the `if_test` matches the condition we care about.

---

_@charliermarsh reviewed on 2024-03-15 02:10_

---

_@ottaviohartman reviewed on 2024-03-15 12:40_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:80 on 2024-03-15 12:40_

Good point - refactored to move this block into the match arm.

---

_Review requested from @charliermarsh by @ottaviohartman on 2024-03-15 12:45_

---

_@charliermarsh approved on 2024-03-18 00:09_

Thanks!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:154 on 2024-03-18 00:10_

@ottaviohartman -- I was able to remove the vector allocation here by making every branch return `&[Stmt]` instead of `&Vec<Stmt>`. Then, you can do a nice trick whereby if you have an `&T`, you can create `&[T]` via `std::slice::from_ref`.

---

_@charliermarsh reviewed on 2024-03-18 00:10_

---

_Label `rule` added by @charliermarsh on 2024-03-18 00:10_

---

_Label `preview` added by @charliermarsh on 2024-03-18 00:10_

---

_Renamed from "`SIM103` detect implicit `else`" to "[`flake8-simplify`] Detect implicit `else` cases in `needless-bool` (`SIM103`)" by @charliermarsh on 2024-03-18 00:10_

---

_Merged by @charliermarsh on 2024-03-18 00:15_

---

_Closed by @charliermarsh on 2024-03-18 00:15_

---

_@ottaviohartman reviewed on 2024-03-18 02:05_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:154 on 2024-03-18 02:05_

Awesome - I'll use that in the future.

---
