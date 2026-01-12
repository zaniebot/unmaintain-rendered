```yaml
number: 10912
title: Limit commutative non-augmented-assignments to primitive data types
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comm
created_at: 2024-04-12T18:46:18Z
updated_at: 2024-04-12T19:02:36Z
url: https://github.com/astral-sh/ruff/pull/10912
synced_at: 2026-01-12T15:55:33Z
```

# Limit commutative non-augmented-assignments to primitive data types

---

_@charliermarsh_

## Summary

I think this is the best we can do without type inference. At least it will still catch some common cases.

Closes #10911.


---

_Label `bug` added by @charliermarsh on 2024-04-12 18:46_

---

_Comment by @github-actions[bot] on 2024-04-12 18:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -109 violations, +0 -0 fixes in 8 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/d9bbcd295832eab0a2da6bf81e0ae00b9777308a/aiven/client/client.py#L159'>aiven/client/client.py:159:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/airflow/providers/apache/hive/hooks/hive.py#L346'>airflow/providers/apache/hive/hooks/hive.py:346:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/airflow/providers/apache/hive/hooks/hive.py#L348'>airflow/providers/apache/hive/hooks/hive.py:348:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/airflow/providers/elasticsearch/log/es_task_handler.py#L207'>airflow/providers/elasticsearch/log/es_task_handler.py:207:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/airflow/providers/microsoft/azure/utils.py#L184'>airflow/providers/microsoft/azure/utils.py:184:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/airflow/utils/operator_helpers.py#L112'>airflow/utils/operator_helpers.py:112:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3196'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3196:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L257'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:257:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L419'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:419:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/apache/airflow/blob/8fad98d34d6493483e934cd76eb82ea97bb58fd1/docs/exts/redirects.py#L70'>docs/exts/redirects.py:70:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/server.py#L294'>src/bokeh/embed/server.py:294:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L307'>src/bokeh/server/tornado.py:307:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L422'>src/bokeh/server/tornado.py:422:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/f8370b1ab5bce469c6ca5a146bc36d8275486245/ibis/backends/druid/__init__.py#L61'>ibis/backends/druid/__init__.py:61:9:</a> PLR6104 Use `|=` to perform an augmented assignment directly
- <a href='https://github.com/ibis-project/ibis/blob/f8370b1ab5bce469c6ca5a146bc36d8275486245/ibis/backends/exasol/__init__.py#L100'>ibis/backends/exasol/__init__.py:100:9:</a> PLR6104 Use `|=` to perform an augmented assignment directly
- <a href='https://github.com/ibis-project/ibis/blob/f8370b1ab5bce469c6ca5a146bc36d8275486245/ibis/common/annotations.py#L287'>ibis/common/annotations.py:287:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/ibis-project/ibis/blob/f8370b1ab5bce469c6ca5a146bc36d8275486245/ibis/common/annotations.py#L289'>ibis/common/annotations.py:289:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -39 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/_testing/contexts.py#L113'>pandas/_testing/contexts.py:113:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/compat/numpy/function.py#L107'>pandas/compat/numpy/function.py:107:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/compat/numpy/function.py#L168'>pandas/compat/numpy/function.py:168:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/compat/numpy/function.py#L200'>pandas/compat/numpy/function.py:200:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/compat/numpy/function.py#L229'>pandas/compat/numpy/function.py:229:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/arrays/masked.py#L693'>pandas/core/arrays/masked.py:693:13:</a> PLR6104 Use `|=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/indexes/datetimes.py#L665'>pandas/core/indexes/datetimes.py:665:13:</a> PLR6104 Use `&=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/indexes/multi.py#L3590'>pandas/core/indexes/multi.py:3590:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/window/numba_.py#L152'>pandas/core/window/numba_.py:152:29:</a> PLR6104 Use `*=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/window/numba_.py#L327'>pandas/core/window/numba_.py:327:29:</a> PLR6104 Use `*=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/window/rolling.py#L827'>pandas/core/window/rolling.py:827:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/window/rolling.py#L828'>pandas/core/window/rolling.py:828:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/core/window/rolling.py#L829'>pandas/core/window/rolling.py:829:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/io/formats/format.py#L344'>pandas/io/formats/format.py:344:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/io/formats/printing.py#L61'>pandas/io/formats/printing.py:61:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/io/formats/xml.py#L231'>pandas/io/formats/xml.py:231:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/pandas-dev/pandas/blob/b9bfc0134455d8a5f269366f87d2a8567aa4269c/pandas/io/formats/xml.py#L234'>pandas/io/formats/xml.py:234:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/chain/bitcoin/bch/utils.py#L132'>rotkehlchen/chain/bitcoin/bch/utils.py:132:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/chain/bitcoin/bch/utils.py#L58'>rotkehlchen/chain/bitcoin/bch/utils.py:58:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/chain/ethereum/graph.py#L71'>rotkehlchen/chain/ethereum/graph.py:71:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/chain/ethereum/modules/eth2/beacon.py#L76'>rotkehlchen/chain/ethereum/modules/eth2/beacon.py:76:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/chain/evm/node_inquirer.py#L301'>rotkehlchen/chain/evm/node_inquirer.py:301:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/accounting_rules.py#L268'>rotkehlchen/db/accounting_rules.py:268:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/addressbook.py#L59'>rotkehlchen/db/addressbook.py:59:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/addressbook.py#L63'>rotkehlchen/db/addressbook.py:63:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/custom_assets.py#L25'>rotkehlchen/db/custom_assets.py:25:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/custom_assets.py#L42'>rotkehlchen/db/custom_assets.py:42:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/dbhandler.py#L2054'>rotkehlchen/db/dbhandler.py:2054:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/rotki/rotki/blob/17b4368bc15043307fa6acf536b5237b3840c40e/rotkehlchen/db/dbhandler.py#L2070'>rotkehlchen/db/dbhandler.py:2070:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/analytics/lib/fixtures.py#L77'>analytics/lib/fixtures.py:77:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/tools/lib/pretty_print.py#L134'>tools/lib/pretty_print.py:134:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/cache.py#L185'>zerver/lib/cache.py:185:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/ccache.py#L43'>zerver/lib/ccache.py:43:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/ccache.py#L45'>zerver/lib/ccache.py:45:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/ccache.py#L70'>zerver/lib/ccache.py:70:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/email_mirror.py#L170'>zerver/lib/email_mirror.py:170:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/generate_test_data.py#L163'>zerver/lib/generate_test_data.py:163:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/url_encoding.py#L108'>zerver/lib/url_encoding.py:108:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/zulip/zulip/blob/436dab0e017f43382519ff5ad0b6bd88e049ed9b/zerver/lib/webhooks/common.py#L168'>zerver/lib/webhooks/common.py:168:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/core/logger.py#L122'>indico/core/logger.py:122:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/attachments/blueprint.py#L100'>indico/modules/attachments/blueprint.py:100:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/attachments/blueprint.py#L59'>indico/modules/attachments/blueprint.py:59:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/attachments/blueprint.py#L61'>indico/modules/attachments/blueprint.py:61:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/attachments/blueprint.py#L98'>indico/modules/attachments/blueprint.py:98:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/events/management/blueprint.py#L79'>indico/modules/events/management/blueprint.py:79:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/events/views.py#L318'>indico/modules/events/views.py:318:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/users/ext.py#L41'>indico/modules/users/ext.py:41:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/modules/users/ext.py#L61'>indico/modules/users/ext.py:61:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
- <a href='https://github.com/indico/indico/blob/cee0ffce26042b11b08c82fffb052944e9459eee/indico/web/flask/util.py#L191'>indico/web/flask/util.py:191:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6104 | 109 | 0 | 109 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-12 19:02_

---

_Closed by @charliermarsh on 2024-04-12 19:02_

---

_Branch deleted on 2024-04-12 19:02_

---

_Comment by @charliermarsh on 2024-04-12 19:02_

Ecosystem checks are good changes.

---
