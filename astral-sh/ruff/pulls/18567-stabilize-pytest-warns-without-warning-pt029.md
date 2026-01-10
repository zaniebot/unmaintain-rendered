```yaml
number: 18567
title: "Stabilize `pytest-warns-without-warning` (`PT029`)"
type: pull_request
state: closed
author: dylwil3
labels:
  - rule
assignees: []
base: brent/release-0.12.0
head: dylan/stabilize-pt029
created_at: 2025-06-08T19:21:10Z
updated_at: 2025-06-09T19:49:30Z
url: https://github.com/astral-sh/ruff/pull/18567
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `pytest-warns-without-warning` (`PT029`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:21_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:21_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:21_

---

_Comment by @github-actions[bot] on 2025-06-08 19:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+238 -80 violations, +8 -0 fixes in 11 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+124 -59 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/api_fastapi/common/parameters.py#L690'>airflow-core/src/airflow/api_fastapi/common/parameters.py:690:23:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/api_fastapi/core_api/routes/public/job.py#L102'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/job.py:102:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/cli/commands/dag_command.py#L207'>airflow-core/src/airflow/cli/commands/dag_command.py:207:14:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/migrations/db_types.pyi#L19'>airflow-core/src/airflow/migrations/db_types.pyi:19:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/migrations/db_types.pyi#L19'>airflow-core/src/airflow/migrations/db_types.pyi:19:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/models/baseoperator.py#L523'>airflow-core/src/airflow/models/baseoperator.py:523:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/models/dagbag.py#L124'>airflow-core/src/airflow/models/dagbag.py:124:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/models/dagbag.py#L125'>airflow-core/src/airflow/models/dagbag.py:125:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/models/taskinstance.py#L1712'>airflow-core/src/airflow/models/taskinstance.py:1712:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/models/taskinstance.py#L1821'>airflow-core/src/airflow/models/taskinstance.py:1821:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/src/airflow/timetables/trigger.py#L55'>airflow-core/src/airflow/timetables/trigger.py:55:32:</a> FBT001 Boolean-typed positional argument in function definition
... 116 additional changes omitted for rule FBT001
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/tests/unit/datasets/test_dataset.py#L76'>airflow-core/tests/unit/datasets/test_dataset.py:76:10:</a> PT029 Set the expected warning in `pytest.warns()`
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/tests/unit/utils/test_process_utils.py#L146'>airflow-core/tests/unit/utils/test_process_utils.py:146:30:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/tests/unit/utils/test_process_utils.py#L152'>airflow-core/tests/unit/utils/test_process_utils.py:152:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/tests/unit/utils/test_process_utils.py#L157'>airflow-core/tests/unit/utils/test_process_utils.py:157:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/tests/unit/utils/test_process_utils.py#L165'>airflow-core/tests/unit/utils/test_process_utils.py:165:25:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/airflow-core/tests/unit/utils/test_process_utils.py#L173'>airflow-core/tests/unit/utils/test_process_utils.py:173:25:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/dev/breeze/src/airflow_breeze/commands/main_command.py#L140'>dev/breeze/src/airflow_breeze/commands/main_command.py:140:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/dev/breeze/src/airflow_breeze/commands/main_command.py#L186'>dev/breeze/src/airflow_breeze/commands/main_command.py:186:27:</a> S603 `subprocess` call: check for execution of untrusted input
... 52 additional changes omitted for rule S603
+ <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi#L21'>task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi:21:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/apache/airflow/blob/3f0e75ea85734861eed94db99d65bd6c1f3f0dfc/task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi#L21'>task-sdk/src/airflow/sdk/definitions/decorators/__init__.pyi:21:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
... 166 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+44 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/RELEASING/changelog.py#L73'>RELEASING/changelog.py:73:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/scripts/change_detector.py#L148'>scripts/change_detector.py:148:65:</a> RUF100 [*] Unused `noqa` directive (unused: `S603`)
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/setup.py#L33'>setup.py:33:73:</a> RUF100 [*] Unused `noqa` directive (unused: `S603`)
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/superset/async_events/async_query_manager.py#L220'>superset/async_events/async_query_manager.py:220:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/superset/commands/dataset/update.py#L61'>superset/commands/dataset/update.py:61:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/superset/common/query_actions.py#L99'>superset/common/query_actions.py:99:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/superset/common/query_context.py#L121'>superset/common/query_context.py:121:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/3256008a59d99b979bf5fd783919de451e13a60a/superset/common/query_context.py#L98'>superset/common/query_context.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
... 37 additional changes omitted for rule FBT001
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+6 -9 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L45'>examples/output/apis/server_document/flask_server.py:45:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L127'>setup.py:127:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L52'>setup.py:52:16:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L186'>src/bokeh/embed/util.py:186:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L609'>src/bokeh/server/tornado.py:609:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L172'>src/bokeh/settings.py:172:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L1'>src/typings/selenium/webdriver/common/action_chains.pyi:1:1:</a> PYI044 [*] `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L1'>src/typings/selenium/webdriver/common/action_chains.pyi:1:1:</a> PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/base.py#L133'>libs/core/langchain_core/language_models/base.py:133:26:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L132'>libs/core/langchain_core/language_models/llms.py:132:20:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L158'>libs/core/langchain_core/language_models/llms.py:158:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L194'>libs/core/langchain_core/language_models/llms.py:194:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L227'>libs/core/langchain_core/language_models/llms.py:227:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L260'>libs/core/langchain_core/language_models/llms.py:260:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/tools/base.py#L667'>libs/core/langchain_core/tools/base.py:667:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/tools/base.py#L779'>libs/core/langchain_core/tools/base.py:779:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L332'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:332:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L356'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:356:5:</a> FBT001 Boolean-typed positional argument in function definition
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7d542b4b429b50f3c6df223275363cad63c9a153/pandas/tests/copy_view/test_chained_assignment_deprecation.py#L20'>pandas/tests/copy_view/test_chained_assignment_deprecation.py:20:10:</a> PT029 Set the expected warning in `pytest.warns()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/732055faaa79efba283a827fab100a50322efe80/rotkehlchen/tests/unit/test_search.py#L60'>rotkehlchen/tests/unit/test_search.py:60:21:</a> S603 `subprocess` call: check for execution of untrusted input
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/skbuild_model.py#L46'>src/scikit_build_core/settings/skbuild_model.py:46:22:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/skbuild_overrides.py#L243'>src/scikit_build_core/settings/skbuild_overrides.py:243:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/skbuild_overrides.py#L244'>src/scikit_build_core/settings/skbuild_overrides.py:244:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/sources.py#L391'>src/scikit_build_core/settings/sources.py:391:14:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/yandex/ch-backup/blob/93f58c514acb5a8f641bd86a576a46654d94e4ba/images/clickhouse/entrypoint.py#L21'>images/clickhouse/entrypoint.py:21:5:</a> S603 `subprocess` call: check for execution of untrusted input
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+33 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/analytics/lib/counts.py#L329'>analytics/lib/counts.py:329:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/corporate/tests/test_stripe.py#L463'>corporate/tests/test_stripe.py:463:9:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/scripts/lib/check_rabbitmq_queue.py#L149'>scripts/lib/check_rabbitmq_queue.py:149:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/scripts/lib/puppet_cache.py#L25'>scripts/lib/puppet_cache.py:25:30:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/scripts/lib/zulip_tools.py#L608'>scripts/lib/zulip_tools.py:608:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/tools/lib/provision.py#L271'>tools/lib/provision.py:271:16:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/tools/lib/provision_inner.py#L109'>tools/lib/provision_inner.py:109:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/tools/lib/test_script.py#L125'>tools/lib/test_script.py:125:5:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/zerver/actions/create_user.py#L512'>zerver/actions/create_user.py:512:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/d1023660da629034b9082d7a3d52ecbdd2392de4/zerver/actions/custom_profile_fields.py#L115'>zerver/actions/custom_profile_fields.py:115:5:</a> FBT001 Boolean-typed positional argument in function definition
... 33 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/_decorators.py#L72'>nox/_decorators.py:72:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/_option_set.py#L166'>nox/_option_set.py:166:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/registry.py#L50'>nox/registry.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/registry.py#L66'>nox/registry.py:66:5:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_c_reader.py#L1175'>astropy/io/ascii/tests/test_c_reader.py:1175:15:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_c_reader.py#L1281'>astropy/io/ascii/tests/test_c_reader.py:1281:15:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_c_reader.py#L1288'>astropy/io/ascii/tests/test_c_reader.py:1288:19:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_c_reader.py#L1352'>astropy/io/ascii/tests/test_c_reader.py:1352:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_c_reader.py#L1363'>astropy/io/ascii/tests/test_c_reader.py:1363:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L513'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:513:18:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/wcs/tests/test_profiling.py#L72'>astropy/wcs/tests/test_profiling.py:72:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/wcs/tests/test_utils.py#L802'>astropy/wcs/tests/test_utils.py:802:15:</a> PT029 Set the expected warning in `pytest.warns()`
</pre>

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 226 | 226 | 0 | 0 | 0 |
| S603 | 80 | 0 | 80 | 0 | 0 |
| PT029 | 10 | 10 | 0 | 0 | 0 |
| PYI044 | 8 | 0 | 0 | 8 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:27_

---

_@ntBre approved on 2025-06-09 19:17_

---

_Comment by @ntBre on 2025-06-09 19:24_

Wait was this changed recently? An empty `pytest.warns()` seems to be working fine for me locally:

```py
# test.py
import pytest
import warnings


def test_foo():
    with pytest.warns():
       warnings.warn("user", UserWarning)
```

---

_Comment by @ntBre on 2025-06-09 19:28_

There's even an example of this in the [docs](https://docs.pytest.org/en/stable/how-to/capture-warnings.html#recwarn) with an `as ...` binding, which is the pattern I saw in the airflow ecosystem check.

---

_Comment by @ntBre on 2025-06-09 19:29_

I guess you can still argue that it's bad style, but the upstream [rationale](https://github.com/m-burst/flake8-pytest-style/blob/v2.1.0/docs/rules/PT029.md#rationale) that it will fail at runtime is not correct in the docs.

---

_Comment by @dscorbett on 2025-06-09 19:35_

`pytest.warns()` raises an error before pytest 7. Since pytest 7, it is the same as `pytest.warns(Warning)`; compare [`pytest-warns-too-broad` (PT030)](https://docs.astral.sh/ruff/rules/pytest-warns-too-broad/).

---

_Comment by @dylwil3 on 2025-06-09 19:45_

Hmm... should we be considering deprecating rather than stabilizing then? And just extend `PT030` to catch this as well? Pytest is now on version 8.4

---

_Comment by @ntBre on 2025-06-09 19:48_

Yeah, I think at least we should not stabilize this for now. I think the change is [here](https://docs.pytest.org/en/stable/changelog.html#pytest-7-0-0-2022-02-03:~:text=%238645%3A%20pytest.warns(None)%20is%20now%20deprecated%20because%20many%20people%20used%20it%20to%20mean%20%E2%80%9Cthis%20code%20does%20not%20emit%20warnings%E2%80%9D%2C%20but%20it%20actually%20had%20the%20effect%20of%20checking%20that%20the%20code%20emits%20at%20least%20one%20warning%20of%20any%20type%20%2D%20like%20pytest.warns()%20or%20pytest.warns(Warning).) in the [pytest 7.0.0rc1](https://docs.pytest.org/en/stable/changelog.html#pytest-7-0-0rc1-2021-12-06) release notes from 2021-12-06 and in this PR: https://github.com/pytest-dev/pytest/issues/8645.

---

_Closed by @dylwil3 on 2025-06-09 19:49_

---
