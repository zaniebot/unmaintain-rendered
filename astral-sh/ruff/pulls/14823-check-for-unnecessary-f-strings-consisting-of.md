```yaml
number: 14823
title: Check for unnecessary f-strings consisting of only one expression
type: pull_request
state: closed
author: lheckemann
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: unnecessary-fstring
created_at: 2024-12-06T16:46:07Z
updated_at: 2024-12-10T16:07:49Z
url: https://github.com/astral-sh/ruff/pull/14823
synced_at: 2026-01-12T15:55:49Z
```

# Check for unnecessary f-strings consisting of only one expression

---

_@lheckemann_

## Summary

Introduce a new check for unnecessary use of f-strings when str() or even using the value directly would be clearer.

The fix unfortunately isn't always sound (and in some situations it's preferable to use the expression directly rather than wrapping it in `str()`), so I've marked it as unsafe.

## Test Plan

Added a snapshot test.

---

_Label `rule` added by @MichaReiser on 2024-12-06 16:51_

---

_Label `preview` added by @MichaReiser on 2024-12-06 16:51_

---

_Comment by @github-actions[bot] on 2024-12-09 11:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+480 -0 violations, +0 -0 fixes in 23 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/test_bot/cogs/slash_commands.py#L67'>test_bot/cogs/slash_commands.py:67:26:</a> RUF056 f-string consisting only of a single replacement field
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/concertbot/actions/actions.py#L15'>examples/concertbot/actions/actions.py:15:39:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/examples/concertbot/actions/actions.py#L30'>examples/concertbot/actions/actions.py:30:39:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/policies/ted_policy.py#L1353'>rasa/core/policies/ted_policy.py:1353:13:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L583'>rasa/shared/core/domain.py:583:22:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/formats/readerwriter.py#L195'>rasa/shared/nlu/training_data/formats/readerwriter.py:195:20:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/training_data.py#L677'>rasa/shared/nlu/training_data/training_data.py:677:22:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/evaluation/test_marker.py#L355'>tests/core/evaluation/test_marker.py:355:53:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_ted_policy.py#L306'>tests/core/policies/test_ted_policy.py:306:13:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_unexpected_intent_policy.py#L1087'>tests/core/policies/test_unexpected_intent_policy.py:1087:27:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/policies/test_unexpected_intent_policy.py#L1195'>tests/core/policies/test_unexpected_intent_policy.py:1195:23:</a> RUF056 f-string consisting only of a single replacement field
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+96 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/example_dags/example_params_ui_tutorial.py#L155'>airflow/example_dags/example_params_ui_tutorial.py:155:17:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/example_dags/example_params_ui_tutorial.py#L163'>airflow/example_dags/example_params_ui_tutorial.py:163:17:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/executors/executor_utils.py#L42'>airflow/executors/executor_utils.py:42:70:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/serialization/dag_dependency.py#L38'>airflow/serialization/dag_dependency.py:38:15:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/serialization/serde.py#L331'>airflow/serialization/serde.py:331:14:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/utils/cli.py#L163'>airflow/utils/cli.py:163:25:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/airflow/www/views.py#L2241'>airflow/www/views.py:2241:19:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/dev/breeze/src/airflow_breeze/commands/developer_commands.py#L919'>dev/breeze/src/airflow_breeze/commands/developer_commands.py:919:80:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L103'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:103:13:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/airflow/blob/ee07d6339a3b1a1b4451c20f5109ab51dcd13ebe/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L204'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:204:38:</a> RUF056 f-string consisting only of a single replacement field
... 86 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+34 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/RELEASING/changelog.py#L326'>RELEASING/changelog.py:326:11:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/RELEASING/changelog.py#L328'>RELEASING/changelog.py:328:11:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/RELEASING/changelog.py#L356'>RELEASING/changelog.py:356:15:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/RELEASING/changelog.py#L363'>RELEASING/changelog.py:363:15:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/superset/cli/main.py#L77'>superset/cli/main.py:77:51:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/superset/cli/main.py#L80'>superset/cli/main.py:80:27:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/superset/databases/utils.py#L58'>superset/databases/utils.py:58:17:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/superset/db_engine_specs/databend.py#L268'>superset/db_engine_specs/databend.py:268:24:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/superset/db_engine_specs/presto.py#L716'>superset/db_engine_specs/presto.py:716:21:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/apache/superset/blob/48c5ee4f8b18d076ce1e0de7c7e7bd92524d6e8c/superset/migrations/shared/security_converge.py#L54'>superset/migrations/shared/security_converge.py:54:16:</a> RUF056 f-string consisting only of a single replacement field
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L26'>examples/server/app/movies/main.py:26:54:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/taylor.py#L43'>examples/server/app/taylor.py:43:28:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/taylor.py#L58'>examples/server/app/taylor.py:58:35:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/descriptors.py#L244'>src/bokeh/core/property/descriptors.py:244:16:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/instance.py#L175'>src/bokeh/core/property/instance.py:175:16:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/primitive.py#L249'>src/bokeh/core/property/primitive.py:249:12:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L85'>src/bokeh/io/webdriver.py:85:57:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L193'>tests/integration/widgets/test_select.py:193:54:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_select.py#L217'>tests/integration/widgets/test_select.py:217:54:</a> RUF056 f-string consisting only of a single replacement field
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py#L37'>docs/how-to/visualization/example_streamlit_app/example_streamlit_app.py:37:31:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/backends/exasol/tests/conftest.py#L118'>ibis/backends/exasol/tests/conftest.py:118:17:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/backends/sql/compilers/datafusion.py#L137'>ibis/backends/sql/compilers/datafusion.py:137:43:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/examples/tests/test_examples.py#L76'>ibis/examples/tests/test_examples.py:76:20:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/expr/decompile.py#L149'>ibis/expr/decompile.py:149:11:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/expr/decompile.py#L159'>ibis/expr/decompile.py:159:11:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/expr/decompile.py#L167'>ibis/expr/decompile.py:167:11:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/expr/decompile.py#L308'>ibis/expr/decompile.py:308:17:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/expr/format.py#L333'>ibis/expr/format.py:333:15:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/ibis-project/ibis/blob/5daddafef488a00645e2abd2fa63a6d1813e8e65/ibis/expr/format.py#L376'>ibis/expr/format.py:376:16:</a> RUF056 f-string consisting only of a single replacement field
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/main.py#L1076'>src/latch_cli/main.py:1076:21:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/get_executions.py#L492'>src/latch_cli/services/get_executions.py:492:26:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/latchbio/latch/blob/d7694f55d377f295472120076c8d7f3d3f0732bd/src/latch_cli/services/k8s/utils.py#L303'>src/latch_cli/services/k8s/utils.py:303:21:</a> RUF056 f-string consisting only of a single replacement field
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L248'>lnbits/commands.py:248:33:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L249'>lnbits/commands.py:249:29:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L250'>lnbits/commands.py:250:29:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/commands.py#L268'>lnbits/commands.py:268:32:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/services.py#L876'>lnbits/core/services.py:876:46:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/payment_api.py#L420'>lnbits/core/views/payment_api.py:420:23:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/user_api.py#L122'>lnbits/core/views/user_api.py:122:20:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/user_api.py#L138'>lnbits/core/views/user_api.py:138:20:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/user_api.py#L158'>lnbits/core/views/user_api.py:158:66:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/user_api.py#L74'>lnbits/core/views/user_api.py:74:20:</a> RUF056 f-string consisting only of a single replacement field
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/a225cbfb66f06e48fce6f350b2948fff6b502e4c/pymilvus/client/grpc_handler.py#L172'>pymilvus/client/grpc_handler.py:172:46:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/milvus-io/pymilvus/blob/a225cbfb66f06e48fce6f350b2948fff6b502e4c/pymilvus/orm/connections.py#L177'>pymilvus/orm/connections.py:177:53:</a> RUF056 f-string consisting only of a single replacement field
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/_testing/asserters.py#L1263'>pandas/_testing/asserters.py:1263:43:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/_testing/asserters.py#L1263'>pandas/_testing/asserters.py:1263:62:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/core/indexes/multi.py#L4207'>pandas/core/indexes/multi.py:4207:16:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/io/formats/style_render.py#L2441'>pandas/io/formats/style_render.py:2441:57:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/io/formats/style_render.py#L2445'>pandas/io/formats/style_render.py:2445:57:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/io/formats/style_render.py#L2480'>pandas/io/formats/style_render.py:2480:32:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/io/formats/style_render.py#L2485'>pandas/io/formats/style_render.py:2485:31:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/io/formats/style_render.py#L2487'>pandas/io/formats/style_render.py:2487:31:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/tests/computation/test_eval.py#L737'>pandas/tests/computation/test_eval.py:737:24:</a> RUF056 f-string consisting only of a single replacement field
+ <a href='https://github.com/pandas-dev/pandas/blob/29d7e0897aa2877a73af173127397e841207e16c/pandas/tests/computation/test_eval.py#L738'>pandas/tests/computation/test_eval.py:738:24:</a> RUF056 f-string consisting only of a single replacement field
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/9c75ea15c2f31a77e6043b80b1b7081372319d85/cibuildwheel/logger.py#L88'>cibuildwheel/logger.py:88:15:</a> RUF056 f-string consisting only of a single replacement field
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/edee8d3157ffc59a33b30166a492261c2d629e14/scripts/sync_protobuf/tensorflow.py#L106'>scripts/sync_protobuf/tensorflow.py:106:13:</a> RUF056 f-string consisting only of a single replacement field
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF056 | 480 | 480 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @lheckemann on 2024-12-09 12:06_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-09 12:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_fstring.rs`:63 on 2024-12-10 08:37_

We should also bail if there's a debug expression (and we should add a test for it): `{expression=}` 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_fstring.rs`:91 on 2024-12-10 08:38_

Nit: You can reduce the nesting by using `let Some(x) = ... else { return }`
```suggestion
    let Some(f_string) = expr.value.as_single() else {
    	return;
  	}
    if f_string.elements.len() != 1 {
        return;
    }
    if let Some(format_expr) = f_string.elements[0].as_expression() {
        if format_expr.format_spec.is_some() {
            return;
        }
        // Edit is unsafe because:
        // - str(foo) may behave differently when `str` is overridden in the local scope.
        // - Empty format specifiers producing the same result as `str` is "A general convention" according to https://docs.python.org/3.13/library/string.html#format-specification-mini-language
        let fix = Fix::unsafe_edits(
            Edit::replacement(
                format!("{}(", conversion_function(&format_expr.conversion)),
                expr.start(),
                format_expr.expression.start(),
            ),
            [Edit::replacement(
                ")".to_string(),
                format_expr.expression.end(),
                expr.end(),
            )],
        );
        checker.diagnostics.push(
            Diagnostic::new(
                UnnecessaryFString {
                    conversion: format_expr.conversion,
                },
                f_string.range(),
            )
            .with_fix(fix),
        );
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_fstring.rs`:27 on 2024-12-10 08:47_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_fstring.rs`:67 on 2024-12-10 08:49_

We could test if `str` resolves to the built-in `str` function in the local scope (or at least, test whether there's an existing binding for `str` that doesn't resolve to a builtin)

https://github.com/astral-sh/ruff/blob/14ba469fc099e0678859bba3ae6f67477d8934ec/crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs#L74-L79

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_fstring.rs`:68 on 2024-12-10 08:50_

We should only mark the fix as unsafe if it the format specifier is empty. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_fstring.rs`:29 on 2024-12-10 08:51_

We should probably use a more specific rule name to distinguish it from https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/ (which you could argue, is also an unnecessary f-string). 

Maybe `single-placeholder-f-string`

---

_@MichaReiser reviewed on 2024-12-10 08:53_

Thanks for working on this rule. 

I'm leaning toward not adding this rule to Ruff right now because it is a stylistic rule. Unless we change the motivation that it is about simplification, but I find it debatable whether `str(expression)` is indeed simpler than `f"{expression}"`. I'd also say that the number of project using f-strings for such single-placeholder-fstrings demonstrate that this is a common and preferred style of many

What are your thoughts on the scope of the rule @AlexWaygood 

---

_Comment by @AlexWaygood on 2024-12-10 13:37_

Thanks for the contribution @lheckemann. However, unfortunately I agree with @MichaReiser on this one. This feels too opinionated for Ruff right now; I think lots of users would find the changes this rule is recommending just to be unnecessary churn for their projects, and reviewing those changes can make using or upgrading Ruff feel like a chore rather than something to be excited about.

While I would personally generally use `str()` for this kind of thing, it's not something I think I would care enough about to want to enforce in CI, and I'm not sure I'd even bother pointing it out in code review. While using an f-string for this might subjectively be less readable for some people, it is more concise, it might actually be faster on older versions of Python (haven't checked), and in situations like [this typeshed ecosystem hit](https://github.com/python/typeshed/blob/edee8d3157ffc59a33b30166a492261c2d629e14/scripts/sync_protobuf/tensorflow.py#L106), for example, it's very nice to have the opening quotes for all the strings aligned on the same column in the page.

At some point we'll [recategorize our rules](https://github.com/astral-sh/ruff/issues/1774), and after we've done that we should be able to more easily add rules like this which are very pedantic in the style they enforce. Until that point, however, there are too many people selecting the `RUF` category or `ALL`, who I think would be negatively impacted by this check being added.

Sorry about this -- I hope you continue to contribute to Ruff! We really do appreciate the PR :-)

---

_Closed by @AlexWaygood on 2024-12-10 13:37_

---

_Branch deleted on 2024-12-10 15:50_

---

_Comment by @lheckemann on 2024-12-10 15:51_

Thanks for the feedback (and especially for the detailed suggestions even though the basic idea is rejected)! I'll subscribe to the linked issue and maybe resubmit when that's happened :)

---

_Branch restored on 2024-12-10 15:51_

---
