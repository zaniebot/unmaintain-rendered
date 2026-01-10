```yaml
number: 11769
title: "Implement  `ForVariableUsedAfterBlock` (based on `wemake_python_styleguide` `ControlVarUsedAfterBlockViolation` `WPS441`)"
type: pull_request
state: open
author: MadLittleMods
labels:
  - rule
  - needs-decision
  - preview
assignees: []
base: main
head: madlittlemods/control-var-used-after-block
created_at: 2024-06-06T01:29:09Z
updated_at: 2025-10-29T15:05:27Z
url: https://github.com/astral-sh/ruff/pull/11769
synced_at: 2026-01-10T16:59:49Z
```

# Implement  `ForVariableUsedAfterBlock` (based on `wemake_python_styleguide` `ControlVarUsedAfterBlockViolation` `WPS441`)

---

_Pull request opened by @MadLittleMods on 2024-06-06 01:29_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add `ForVariableUsedAfterBlock` (`RUF032`) based on [`ControlVarUsedAfterBlockViolation` (`WPS441`)](https://wemake-python-styleguide.readthedocs.io/en/0.19.2/pages/usage/violations/best_practices.html#wemake_python_styleguide.violations.best_practices.ControlVarUsedAfterBlockViolation) from the `wemake_python_styleguide`. This rule prevents you from using `for`-loop variables outside of the loop block ~~(or a `with` context variable)~~.

Example:

```py
# For control var used outside block
for event in []:
    _ = event
    pass

# ❌
_ = event
```

<details>
<summary><s><code>with</code> statement</s></summary>

```
# with statement
with None as w:
    _ = w
    pass

# ❌
_ = w
```

</details>

Part of https://github.com/astral-sh/ruff/issues/3845

This is spawning from a real-world pain where I accidentally used one for-loop control variable in another loop, see https://github.com/element-hq/synapse/pull/17187#discussion_r1612211662

Relevant wtfPython around loop variables leaking out, https://github.com/satwikkansal/wtfPython?tab=readme-ov-file#-loop-variables-leaking-out

Please bear in mind that this is my first Rust code so I'm probably doing a few things wrong :bow:. Shout out to @reivilibre who helped me in the beginning!


## Test Plan

<!-- How was it tested? -->

Tested against the fixture file:
```
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py --no-cache --select RUF032 --preview
```

<details>
<summary>Previous command before the rename</summary>

```
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/wemake_python_styleguide/control_var_used_after_block.py --no-cache --select WPS441
```

</details>

Also tested against the https://github.com/element-hq/synapse codebase. It does trigger in several spots that work because Python works this way but could be refactored to align with this rule. Some of the usages seem slightly sketchy but it requires some more context/time to determine whether they are bugs in the Synapse code.

Here is a real world bug that it caught: https://github.com/element-hq/synapse/pull/17272

### Dev notes

Running `ruff` against your test file with your new rule:

```
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/wemake_python_styleguide/control_var_used_after_block.py --no-cache --select WPS441
```


#### Getting started

 - How to get the project running -> [`CONTRIBUTING.md#prerequisites`](https://github.com/astral-sh/ruff/blob/084e5464fb785f3f8fcdc54505b954eae6886906/CONTRIBUTING.md#prerequisites)
 - How to add a new lint rule -> [`CONTRIBUTING.md#example-adding-a-new-lint-rule`](https://github.com/astral-sh/ruff/blob/084e5464fb785f3f8fcdc54505b954eae6886906/CONTRIBUTING.md#example-adding-a-new-lint-rule)

Running the `pre-commit` hooks without installing to your system:

```sh
python -m venv /home/eric/Downloads/ruff-venv
source /home/eric/Downloads/ruff-venv/bin/activate
pip install pre-commit
```


#### Debug `NodeId`/`node_id`

Add the following method to `crates/ruff_python_semantic/src/model.rs` and use `println!("nodes {:#?}", checker.semantic().nodes());`. It would be really nice if the node index/ID was printed next to each one but you can manually number things to get by.
```
    #[inline]
    pub fn nodes(&self) -> &Nodes<'a> {
        &self.nodes
    }
```

#### Inspect the AST

```
cargo dev print-ast crates/ruff_linter/resources/test/fixtures/wemake_python_styleguide/control_var_used_after_block.py
```


#### Helpful related `ruff` rules

 - `unused_variable`
 - `redefined_loop_name`
 - `too_many_nested_blocks`
 - `unused_loop_control_variable`

---

_Comment by @github-actions[bot] on 2024-06-06 01:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+117 -0 violations, +0 -0 fixes in 15 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/dd1e6aab5ff89e15d509b1672431db2f41d02425/disnake/message.py#L1104'>disnake/message.py:1104:28:</a> RUF032 `for` loop variable index is used outside of block
+ <a href='https://github.com/DisnakeDev/disnake/blob/dd1e6aab5ff89e15d509b1672431db2f41d02425/disnake/message.py#L1105'>disnake/message.py:1105:16:</a> RUF032 `for` loop variable reaction is used outside of block
+ <a href='https://github.com/DisnakeDev/disnake/blob/dd1e6aab5ff89e15d509b1672431db2f41d02425/scripts/codemods/typed_permissions.py#L84'>scripts/codemods/typed_permissions.py:84:33:</a> RUF032 `for` loop variable b is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/nlu/training_data/test_features.py#L234'>tests/shared/nlu/training_data/test_features.py:234:46:</a> RUF032 `for` loop variable idx is used outside of block
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/shared/nlu/training_data/test_features.py#L234'>tests/shared/nlu/training_data/test_features.py:234:67:</a> RUF032 `for` loop variable idx is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+25 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/api/__init__.py#L46'>airflow/api/__init__.py:46:76:</a> RUF032 `for` loop variable backend is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/models/param.py#L284'>airflow/models/param.py:284:67:</a> RUF032 `for` loop variable k is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/amazon/aws/sensors/s3.py#L150'>airflow/providers/amazon/aws/sensors/s3.py:150:64:</a> RUF032 `for` loop variable key is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/amazon/aws/sensors/s3.py#L154'>airflow/providers/amazon/aws/sensors/s3.py:154:41:</a> RUF032 `for` loop variable key is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/amazon/aws/sensors/s3.py#L159'>airflow/providers/amazon/aws/sensors/s3.py:159:50:</a> RUF032 `for` loop variable key is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/apache/hive/hooks/hive.py#L1015'>airflow/providers/apache/hive/hooks/hive.py:1015:59:</a> RUF032 `for` loop variable i is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L476'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:476:60:</a> RUF032 `for` loop variable line is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L479'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:479:60:</a> RUF032 `for` loop variable line is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/providers/docker/operators/docker_swarm.py#L221'>airflow/providers/docker/operators/docker_swarm.py:221:32:</a> RUF032 `for` loop variable line is used outside of block
+ <a href='https://github.com/apache/airflow/blob/aa9bb4bb47878c2f4aec0df58a2edbe0e1e1f797/airflow/timetables/events.py#L103'>airflow/timetables/events.py:103:47:</a> RUF032 `for` loop variable next_event is used outside of block
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/cli/test_db.py#L235'>superset/cli/test_db.py:235:8:</a> RUF032 `for` loop variable spec is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/cli/test_db.py#L238'>superset/cli/test_db.py:238:21:</a> RUF032 `for` loop variable spec is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/cli/test_db.py#L269'>superset/cli/test_db.py:269:12:</a> RUF032 `for` loop variable spec is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/migrations/versions/2017-10-03_14-37_4736ec66ce19_.py#L204'>superset/migrations/versions/2017-10-03_14-37_4736ec66ce19_.py:204:19:</a> RUF032 `for` loop variable foreign is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/migrations/versions/2020-08-12_00-24_978245563a02_migrate_iframe_to_dash_markdown.py#L191'>superset/migrations/versions/2020-08-12_00-24_978245563a02_migrate_iframe_to_dash_markdown.py:191:40:</a> RUF032 `for` loop variable dashboard is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/sql_parse.py#L1539'>superset/sql_parse.py:1539:53:</a> RUF032 `for` loop variable dialect is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/tasks/cache.py#L269'>superset/tasks/cache.py:269:31:</a> RUF032 `for` loop variable class_ is used outside of block
+ <a href='https://github.com/apache/superset/blob/38d64e8dd2a3e1ec5e67bdbf062054b4188988d8/superset/tasks/cache.py#L271'>superset/tasks/cache.py:271:20:</a> RUF032 `for` loop variable class_ is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/document_lifecycle.py#L70'>src/bokeh/application/handlers/document_lifecycle.py:70:13:</a> RUF032 `for` loop variable callback is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L276'>src/bokeh/embed/util.py:276:27:</a> RUF032 `for` loop variable elementid is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L276'>src/bokeh/embed/util.py:276:38:</a> RUF032 `for` loop variable root is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L276'>src/bokeh/embed/util.py:276:47:</a> RUF032 `for` loop variable root is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L276'>src/bokeh/embed/util.py:276:58:</a> RUF032 `for` loop variable root is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L483'>src/bokeh/plotting/_figure.py:483:13:</a> RUF032 `for` loop variable kw is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L484'>src/bokeh/plotting/_figure.py:484:46:</a> RUF032 `for` loop variable kw is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L488'>src/bokeh/plotting/_figure.py:488:35:</a> RUF032 `for` loop variable kw is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L116'>src/bokeh/util/compiler.py:116:38:</a> RUF032 `for` loop variable i is used outside of block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_code_quality.py#L112'>tests/codebase/test_code_quality.py:112:12:</a> RUF032 `for` loop variable line is used outside of block
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/ae6e38a23fcbde72aae1bf6784ebb4afc413a0ad/ibis/backends/bigquery/tests/unit/udf/test_core.py#L181'>ibis/backends/bigquery/tests/unit/udf/test_core.py:181:16:</a> RUF032 `for` loop variable i is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/milvus_client/rbac.py#L97'>examples/milvus_client/rbac.py:97:17:</a> RUF032 `for` loop variable role is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/__init__.py#L19'>pandas/__init__.py:19:25:</a> RUF032 `for` loop variable _dependency is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/common.py#L599'>pandas/io/common.py:599:8:</a> RUF032 `for` loop variable compression is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/common.py#L600'>pandas/io/common.py:600:16:</a> RUF032 `for` loop variable compression is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/common.py#L604'>pandas/io/common.py:604:43:</a> RUF032 `for` loop variable compression is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/formats/excel.py#L668'>pandas/io/formats/excel.py:668:25:</a> RUF032 `for` loop variable lnum is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/formats/excel.py#L673'>pandas/io/formats/excel.py:673:29:</a> RUF032 `for` loop variable lnum is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/formats/excel.py#L678'>pandas/io/formats/excel.py:678:27:</a> RUF032 `for` loop variable lnum is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/plotting/_matplotlib/boxplot.py#L557'>pandas/plotting/_matplotlib/boxplot.py:557:16:</a> RUF032 `for` loop variable ax is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/tests/frame/indexing/test_take.py#L48'>pandas/tests/frame/indexing/test_take.py:48:13:</a> RUF032 `for` loop variable df is used outside of block
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/tests/frame/indexing/test_take.py#L50'>pandas/tests/frame/indexing/test_take.py:50:13:</a> RUF032 `for` loop variable df is used outside of block
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/masonry/builders/editable.py#L182'>src/poetry/masonry/builders/editable.py:182:27:</a> RUF032 `for` loop variable scripts_path is used outside of block
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/masonry/builders/editable.py#L184'>src/poetry/masonry/builders/editable.py:184:64:</a> RUF032 `for` loop variable scripts_path is used outside of block
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/masonry/builders/editable.py#L207'>src/poetry/masonry/builders/editable.py:207:28:</a> RUF032 `for` loop variable scripts_path is used outside of block
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/utils/env/test_env.py#L303'>tests/utils/env/test_env.py:303:37:</a> RUF032 `for` loop variable dist is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/api/rest.py#L559'>rotkehlchen/api/rest.py:559:49:</a> RUF032 `for` loop variable idx is used outside of block
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/chain/zksync_lite/manager.py#L828'>rotkehlchen/chain/zksync_lite/manager.py:828:25:</a> RUF032 `for` loop variable target is used outside of block
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/tests/db/test_db_upgrades.py#L2603'>rotkehlchen/tests/db/test_db_upgrades.py:2603:12:</a> RUF032 `for` loop variable idx is used outside of block
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/tests/integration/test_zksynclite.py#L108'>rotkehlchen/tests/integration/test_zksynclite.py:108:25:</a> RUF032 `for` loop variable idx is used outside of block
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/tests/integration/test_zksynclite.py#L91'>rotkehlchen/tests/integration/test_zksynclite.py:91:25:</a> RUF032 `for` loop variable idx is used outside of block
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L329'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py:329:75:</a> RUF032 `for` loop variable key is used outside of block
+ <a href='https://github.com/rotki/rotki/blob/3e540b04d9e12d01abbb33d1ae3af0c90014ddbb/rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py#L329'>rotkehlchen/tests/unit/globaldb/test_globaldb_consistency.py:329:84:</a> RUF032 `for` loop variable packaged_field is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/actions/streams.py#L628'>zerver/actions/streams.py:628:16:</a> RUF032 `for` loop variable stream_id is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/actions/streams.py#L631'>zerver/actions/streams.py:631:71:</a> RUF032 `for` loop variable stream_id is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/lib/markdown/__init__.py#L1036'>zerver/lib/markdown/__init__.py:1036:75:</a> RUF032 `for` loop variable size_name is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_auth_backends.py#L239'>zerver/tests/test_auth_backends.py:239:32:</a> RUF032 `for` loop variable backend_name is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_auth_backends.py#L256'>zerver/tests/test_auth_backends.py:256:32:</a> RUF032 `for` loop variable backend_name is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_message_send.py#L1686'>zerver/tests/test_message_send.py:1686:30:</a> RUF032 `for` loop variable user is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_message_send.py#L1699'>zerver/tests/test_message_send.py:1699:30:</a> RUF032 `for` loop variable user is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_user_topics.py#L124'>zerver/tests/test_user_topics.py:124:44:</a> RUF032 `for` loop variable data is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_user_topics.py#L129'>zerver/tests/test_user_topics.py:129:9:</a> RUF032 `for` loop variable data is used outside of block
+ <a href='https://github.com/zulip/zulip/blob/40f59a05c55e0e4f26ca87d2bca646770e94bff0/zerver/tests/test_user_topics.py#L131'>zerver/tests/test_user_topics.py:131:48:</a> RUF032 `for` loop variable data is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/a22e0c0a9c0cf4d40d04029489dc530b4819d2ea/bin/utils/storage_checksums.py#L43'>bin/utils/storage_checksums.py:43:12:</a> RUF032 `for` loop variable row is used outside of block
+ <a href='https://github.com/indico/indico/blob/a22e0c0a9c0cf4d40d04029489dc530b4819d2ea/bin/utils/storage_checksums.py#L46'>bin/utils/storage_checksums.py:46:57:</a> RUF032 `for` loop variable row is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/c9baca6045d427fd4cc0de2ca396ddeb4a0f40a5/notes-to-self/socketpair-buffering.py#L35'>notes-to-self/socketpair-buffering.py:35:44:</a> RUF032 `for` loop variable _count is used outside of block
+ <a href='https://github.com/python-trio/trio/blob/c9baca6045d427fd4cc0de2ca396ddeb4a0f40a5/src/trio/_tests/check_type_completeness.py#L106'>src/trio/_tests/check_type_completeness.py:106:90:</a> RUF032 `for` loop variable obj_name is used outside of block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492'>src/_pytest/assertion/rewrite.py:492:40:</a> RUF032 `for` loop variable i is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492'>src/_pytest/assertion/rewrite.py:492:53:</a> RUF032 `for` loop variable i is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492'>src/_pytest/assertion/rewrite.py:492:66:</a> RUF032 `for` loop variable i is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L495'>src/_pytest/assertion/rewrite.py:495:12:</a> RUF032 `for` loop variable expl is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L713'>src/_pytest/assertion/rewrite.py:713:23:</a> RUF032 `for` loop variable item is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L713'>src/_pytest/assertion/rewrite.py:713:50:</a> RUF032 `for` loop variable item is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L714'>src/_pytest/assertion/rewrite.py:714:22:</a> RUF032 `for` loop variable item is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L716'>src/_pytest/assertion/rewrite.py:716:22:</a> RUF032 `for` loop variable item is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/truncate.py#L111'>src/_pytest/assertion/truncate.py:111:37:</a> RUF032 `for` loop variable iterated_index is used outside of block
+ <a href='https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/truncate.py#L112'>src/_pytest/assertion/truncate.py:112:30:</a> RUF032 `for` loop variable iterated_index is used outside of block
... 10 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF032 | 117 | 117 | 0 | 0 | 0 |

</p>
</details>




---

_@MadLittleMods reviewed on 2024-07-23 02:21_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/wemake_python_styleguide/control_var_used_after_block.py`:1 on 2024-07-23 02:21_

Friendly ping @charliermarsh :bow: 

Just looking for the next steps here (whether that be in a queue, pinging another person, etc)

---

_@MichaReiser reviewed on 2024-07-24 13:22_

Thanks for the contribution. 

What's the motivation for limiting the rule to `with` and `for` only? Should the rule apply to all compound statements? 

@AlexWaygood mentioned that there reasons to access variables from inside a `with` block. So maybe we should exclude `with` statements.

---

_@MadLittleMods reviewed on 2024-07-24 18:37_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/wemake_python_styleguide/control_var_used_after_block.py`:1 on 2024-07-24 18:37_

Thanks for the reply!

> What's the motivation for limiting the rule to `with` and `for` only? Should the rule apply to all compound statements?
>
> *-- @MichaReiser, https://github.com/astral-sh/ruff/pull/11769#pullrequestreview-2196737675*

Ideally, I would like to lint for full block-scoping in Python.

Since Python isn't block-scoped, people often write code with patterns that go against this so there would be a lot of violations in existing codebases for full block-scoping. Using a loop-var after the block is *probably* a mistake though (or at the very least requires a second-look to make sure you're doing it right in your new code) and is a good thing to watch out for in any case (good introduction).

But probably the main motivation for why only `with`/`for` is just to match what the `wemake-python-styleguide` did (prior art). Given the "drop-in" nature that `ruff` seems to try to sit in, I thought I would match this.

> @AlexWaygood mentioned that there reasons to access variables from **inside** a with block. So maybe we should exclude with statements.

Inside should be fine. I assume you typo'ed and meant `ouside` there?

I guess it would be good to know the reasons/use cases to judge better but there are use cases even for the for-loop behavior but the goal is just to lint against that regardless in favor of refactoring to a better pattern.

And if we don't apply this to `with` loops, it's not a drop-in replacement for the other rule. But perhaps that's not a strict goal, etc


---

_Comment by @MichaReiser on 2024-07-29 06:46_

We discussed this internally and we agreed on:

* Using the `for` introduced variable outside of a loop is likely a bug. There are cases where this is intentional, but that's when using suppressions is fine. 
* Using the `with` introduced variable after the `with` block is more common and often acceptable. @AlexWaygood would you mind sharing an example?
* There's a similar problem related to variables introduced by `match case` 
  ```python
  import random

  match random.choice([True, False]):
      case True: False
      case x:
          y = 2


  print(x)
  ```


The fact that `for` is most likely an error and `with` is not justifies that this rule should be split into two rules. I suggest that we start with the `for` loop and add it to the `RUF` ruleset. 

---

_Comment by @AlexWaygood on 2024-07-29 13:12_

> Using the `with` introduced variable after the `with` block is more common and often acceptable. @AlexWaygood would you mind sharing an example?

A commit merged to the CPython repo earlier this morning has several examples in it: https://github.com/python/cpython/commit/0697188084bf61b28f258fbbe867e1010d679b3e.

Here's a slightly simpler example than any of the functions that commit touched, from CPython's `dataclasses` test suite: https://github.com/python/cpython/blob/9187484dd97f6beb94fc17676014706922e380e1/Lib/test/test_dataclasses/__init__.py#L3121C1-L3138C68:

```py
import unittest
from dataclasses import dataclass

class FrozenTests(unittest.TestCase):
    def test_non_frozen_normal_derived_from_empty_frozen(self):
        @dataclass(frozen=True)
        class D:
            pass

        class S(D):
            pass

        s = S()
        self.assertFalse(hasattr(s, 'x'))
        s.x = 5
        self.assertEqual(s.x, 5)

        del s.x
        self.assertFalse(hasattr(s, 'x'))
        with self.assertRaises(AttributeError) as cm:
            del s.x
        self.assertNotIsInstance(cm.exception, FrozenInstanceError)
```

The `with self.assertRaises` block is kept as small as possible, because the author of the test wanted to assert that the `AttributeError` exception is raised on exactly one line: the `del s.x` call. If an `AttributeError` was raised on another line, that would indicate a bug in the test or in the library; that should fail. So the `with` block is only one line long; but you want to save the `cm` binding that's created in the `with` statement so that you can do some extra assertions on it after the `with` block has ended.

I think tests are the places where this comes up most often. You also see this pattern a lot with `pytest`-style tests that use `with pytest.raises()` context managers; it's not just `unittest`-style tests.

---

_Comment by @MadLittleMods on 2024-07-30 04:07_

> I suggest that we start with the `for` loop and add it to the `RUF` ruleset.

Updated :white_check_mark: 

---

_Renamed from "[`wemake_python_styleguide`] Implement  `ControlVarUsedAfterBlockViolation` (`WPS441`)" to "Implement  `ControlVarUsedAfterBlockViolation` (based on `wemake_python_styleguide` `WPS441`)" by @MadLittleMods on 2024-07-30 04:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:56 on 2024-07-30 06:47_

I think we can remove `BlockKind` now that we only support a single block

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:958 on 2024-07-30 06:48_

We should put the rule into the preview group. It allows us to collect feedback before making the rule available to everyone
```suggestion
        (Ruff, "031") => (RuleGroup::Preview, rules::ruff::rules::ControlVarUsedAfterBlock),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:91 on 2024-07-30 06:52_

```suggestion
		let loop_var_bindings = reversed_bindings
        .map(|(name, binding_id)| (name, checker.semantic().binding(*binding_id)))
        .filter_map(|(name, binding)| {
            binding.kind.is_loop_var().then_some((name, binding))
        });
		
    for (&name, binding) in loop_var_bindings {
```

I think it would help readability if the `in` expression is extracted into its own variable

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:76 on 2024-07-30 06:53_

Collecting all bindings seems expensive. From what i understand, this is necessary because `all_bindings` doesn't implement `DoubleEndedIterator`. 

You explain that we have to iterate over the bindings in reverse order. To me it's not entirely clear which logic depends on it. Could you point it out to me?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:94 on 2024-07-30 06:55_

I'm a bit scared about the many `unwrap` calls in this block. Why is it safe to call `unwrap` for all these?  Could we add a comment explaining the safety constraints?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:113 on 2024-07-30 06:56_

Collecting here seems unnecessary since we're only iterating over the entries in the for loop below
```suggestion
            let statement_hierarchy: Vec<NodeId> = checker
                .semantic()
                .parent_statement_ids(reference_node_id);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:36 on 2024-07-30 06:59_

I'm inclined to rename the rule to `ForVariableUsedAfterBlock`, now that the rule is specific to for loops

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:122 on 2024-07-30 07:02_

Can we add an example where the for loop contains a nested function and it access the variable

```python
for x in range(0..10):
	def test(): 
		print(x)

	class Test:
		a = x
```

What's the expected output in this case? 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:120 on 2024-07-30 07:04_

Nit: You could use a label for the outer loop and break the outer loop. It removes the need to use the `is_used_in_block` variable.

```rust
'references for reference in binding.references() {
	...
	
	if binding_statement_id == ancestor_node_id {
		break 'references;
	}

	...
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1210 on 2024-07-30 07:07_

The `statement` function could now make use of `statement_id`

---

_@MichaReiser reviewed on 2024-07-30 07:08_

Overall looking good. It would be great if we could avoid the collection of the bindings. I think they are the root cause for the 1-2% performance regression in the benchmarks.

---

_@MichaReiser requested changes on 2024-08-01 21:40_

We should release this new rule in preview mode first and try to improve its performance (maybe that's not possible, that's okay too)

---

_@MadLittleMods reviewed on 2024-08-02 07:11_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:76 on 2024-08-02 07:11_

Using the following example from the `control_var_used_after_block.py` fixture file:

 - The `references` for the `1 binding` are just the `1 reference`
 - The `references` for the `2 binding` are `1 reference` and `2 reference`
 
```py
# 1 binding
for x in []:
    # 1 reference
    _ = x
    pass

# 2 binding
for x in []:
    # 2 reference
    _ = x
    pass
```

We want to loop over the bindings in source-order so as we find `references` in the block, we can mark them as `known_good_reference_node_ids`. This way we see the `1 binding` first, which will mark `1 reference` as a `known_good_reference_node_ids` so when we process the `2 binding`, we ignore `1 reference` and avoid a false violation.

Reversing the iteration gets us source-order.

---

Updated the comment there to try to explain this better :+1: 


---

_@MadLittleMods reviewed on 2024-08-02 08:18_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:94 on 2024-08-02 08:18_

I feel like I might need some help on these.

### 1. `binding.statement(checker.semantic()).unwrap();`

What scenarios would a variable `binding.statement` be `None`? Feel like this might be a situation where it's a problem with `ruff` itself and we should give up. How do I represent that?


### 2. `binding.source.unwrap();`

I think we're safe here as how can you bind a variable without the `source`? This again feels it would be a problem with `ruff` itself and we should give up. How do I represent that?

Looking wherever we create a new `Binding { ... }`, it seems like [`source` is `None`  with `builtin`'s](https://github.com/astral-sh/ruff/blob/2e2b1b460f4de25e630cde69b4b49577bc5134b4/crates/ruff_python_semantic/src/model.rs#L229-L241).

I don't think it's possible for us to deal with variable bindings for a `builtin`? (please double-check with your own knowledge)

Although I guess the following example works in the Python interpreter but it doesn't seem to pop up as a variable `binding` at all in `ruff`:

```python
import builtins

for builtins.x in [1,2,3]:
    _ = x
    pass

# 3
print(x)
```

### 3. `reference.expression_id().unwrap()`

This one says:

[`reference.rs#L16-L18`](https://github.com/astral-sh/ruff/blob/2e2b1b460f4de25e630cde69b4b49577bc5134b4/crates/ruff_python_semantic/src/reference.rs#L16-L18)
```
/// The expression that the reference occurs in. `None` if the reference is a global
/// reference or a reference via an augmented assignment.
```

I'm not sure what it means by "global reference" as it seems to work fine with our `global_var` example in the fixture file.

I don't think we can run into an [augmented assignment](https://docs.python.org/3/reference/simple_stmts.html#augmented-assignment-statements) (`x += 1`) in the for-loop binding syntax? (please double-check with your own knowledge)

It seems like this can only be `None` if the `SemanticModel.node_id` is [`None` which happens on on `new(...)`](https://github.com/astral-sh/ruff/blob/2e2b1b460f4de25e630cde69b4b49577bc5134b4/crates/ruff_python_semantic/src/model.rs#L150-L155) and gets filled in as soon as we `push_node` something onto the stack. If we're iterating over any source code, it feels like something would be pushed onto the stack.


---

_@MadLittleMods reviewed on 2024-08-02 09:14_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:122 on 2024-08-02 09:14_

Added :white_check_mark: 

No violations are thrown for that example. I think that's as expected given this sort of usage :thinking: 

```python
for x in range(0, 10):
    def test(): 
        print(x)
        
    test()
        
    class Test:
        a = x
        
    foo = Test()
    print(f"foo {foo.a}")
```

---

_Review requested from @MichaReiser by @MadLittleMods on 2024-08-02 09:25_

---

_Comment by @codspeed-hq[bot] on 2024-08-02 09:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/MadLittleMods%3Amadlittlemods%2Fcontrol-var-used-after-block?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #11769 will **degrade performances by 5.26%**

<sub>Comparing <code>MadLittleMods:madlittlemods/control-var-used-after-block</code> (9bcc4fe) with <code>main</code> (c906b01)</sub>



### Summary

`❌ 1` regression  
`✅ 31` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/MadLittleMods%3Amadlittlemods%2Fcontrol-var-used-after-block?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` linter/all-rules[numpy/globals.py] ``](https://codspeed.io/astral-sh/ruff/branches/MadLittleMods%3Amadlittlemods%2Fcontrol-var-used-after-block?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bnumpy%2Fglobals.py%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 726.2 µs | 766.5 µs | -5.26% |


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:107 on 2024-08-08 08:29_

When I remove the `known_good_reference_node_ids` check, then no test fails. Can we add a test that demonstrates why the check is necesseary or remove the check?
```suggestion
            diagnostics.push(Diagnostic::new(
                ForVariableUsedAfterBlock {
                    control_var_name: name.to_owned(),
                },
                reference.range(),
            ));
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:63 on 2024-08-08 08:32_

I was able to remove the `rev` call and all tests keep passing. We should add a test that demonstrate why `rev` is necessary or remove the `rev` call (and the collecting of all bindings) if reversing in reversed-source-order is unnecessary.

I think if reverse iterating matters (because of redefinitions), then I'm not sure if only iterating over the `loop_var_bindings` is sufficient in the next step (what if the redeclaration is a non-loop variable)?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:71 on 2024-08-08 08:34_

According to the documentation, the expression can be `None` when

```
    /// The expression that the reference occurs in. `None` if the reference is a global
    /// reference or a reference via an augmented assignment.
    ```

As far as I can tell, this doesn't apply here. It would be good to add a comment here explaining why calling `unwrap` is safe.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:71 on 2024-08-08 08:37_

The `statement` can be `None` if the binding comes from a built in. Let's add a comment explaining that calling `unwrap` is ok, because the rule only looks at for loop bindings.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:75 on 2024-08-08 08:40_

We can reduce the number of unwraps by using `binding_source_node_id`

```suggestion
        let binding_source_node_id = binding.source.unwrap();
        // The node_id of the for-loop that contains the binding
        let binding_statement_id = checker.semantic().statement_id(binding_source_node_id);
        let binding_statement = checker.semantic().statement(binding_source_node_id);
```

---

_@MichaReiser reviewed on 2024-08-08 08:40_

---

_@MadLittleMods reviewed on 2024-08-09 05:22_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:107 on 2024-08-09 05:22_

Good call on this :thumbsup: The `known_good_reference_node_ids` evolved first to [solve the shadowed binding cases](https://github.com/astral-sh/ruff/commit/eaf2a35753b8bb3777d455be8e336a3072887f16) but my [fix later on to allow previous references](https://github.com/astral-sh/ruff/commit/a7de1cb892d3ca34b35fb90daeae2367465e99b4) also ended up covering the shadowed cases without me realizing (much more elegant too). Sorry for not noticing :grimacing:

Removed the `known_good_reference_node_ids` logic :white_check_mark: 

---

_@MadLittleMods reviewed on 2024-08-09 05:23_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:63 on 2024-08-09 05:23_

Removed the reverse iteration over the bindings :white_check_mark: . This was only necessary to track `known_good_reference_node_ids` properly but [since that's not necessary anymore](https://github.com/astral-sh/ruff/pull/11769#discussion_r1710766228), can be removed.

---

_@MadLittleMods reviewed on 2024-08-09 05:29_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:76 on 2024-08-09 05:29_

Conversation continued in these discussions:

 - https://github.com/astral-sh/ruff/pull/11769#discussion_r1708943441
 - https://github.com/astral-sh/ruff/pull/11769#discussion_r1708948858

---

_@MadLittleMods reviewed on 2024-08-09 05:33_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:71 on 2024-08-09 05:33_

This one is no longer necessary since we can avoid the `unwrap()` entirely thanks to your [suggestion below](https://github.com/astral-sh/ruff/pull/11769#discussion_r1708963581)

See https://github.com/astral-sh/ruff/pull/11769#discussion_r1696409339 for more discussion on the `unwrap()` calls

---

_Label `rule` added by @dhruvmanila on 2024-08-09 05:36_

---

_Label `preview` added by @dhruvmanila on 2024-08-09 05:36_

---

_@MadLittleMods reviewed on 2024-08-09 05:38_

---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/control_var_used_after_block.rs`:94 on 2024-08-09 05:38_

### 1. `binding.statement(checker.semantic()).unwrap();`

Answered in a discussion below:

> The `statement` can be `None` if the binding comes from a built in. Let's add a comment explaining that calling `unwrap` is ok, because the rule only looks at for loop bindings.
>
> *-- https://github.com/astral-sh/ruff/pull/11769#discussion_r1708959174*

Thankfully, we can avoid this `unwrap()` altogether if we grab the `binding_statement` a different way (see https://github.com/astral-sh/ruff/pull/11769#discussion_r1708963581)


### 2. `binding.source.unwrap();`

(no discussion yet)

### 3. `reference.expression_id().unwrap()`

Answered in a discussion below which seems in line with what I was finding.

> According to the documentation, the expression can be `None` when
> 
> ```
> /// The expression that the reference occurs in. `None` if the reference is a global
> /// reference or a reference via an augmented assignment.
> ```
> 
> As far as I can tell, this doesn't apply here. It would be good to add a comment here explaining why calling `unwrap` is safe.
> 
> *-- https://github.com/astral-sh/ruff/pull/11769#discussion_r1708953364*


---

_Review comment by @MadLittleMods on `crates/ruff_linter/src/rules/ruff/rules/for_variable_used_after_block.rs`:71 on 2024-08-09 05:39_

Added a comment :white_check_mark: 

See https://github.com/astral-sh/ruff/pull/11769#discussion_r1696409339 for more discussion on the `unwrap()` calls

---

_@MadLittleMods reviewed on 2024-08-09 05:39_

---

_Renamed from "Implement  `ControlVarUsedAfterBlockViolation` (based on `wemake_python_styleguide` `WPS441`)" to "Implement  `ForVariableUsedAfterBlock` (based on `wemake_python_styleguide` `WPS441`)" by @MadLittleMods on 2024-08-09 06:17_

---

_Renamed from "Implement  `ForVariableUsedAfterBlock` (based on `wemake_python_styleguide` `WPS441`)" to "Implement  `ForVariableUsedAfterBlock` (based on `wemake_python_styleguide` `ControlVarUsedAfterBlockViolation` `WPS441`)" by @MadLittleMods on 2024-08-09 06:18_

---

_Review requested from @MichaReiser by @MadLittleMods on 2024-08-09 06:48_

---

_Review comment by @MadLittleMods on `crates/ruff_python_semantic/src/binding.rs`:29 on 2024-08-09 06:50_

Please double-check the accuracy of these comments. In any case, I'd like to document the cases so we don't have to run through this uncertainty in the future.

---

_@MadLittleMods reviewed on 2024-08-09 06:50_

---

_Comment by @MichaReiser on 2024-08-09 08:14_

Some interesting cases from the ecosystem check

**Variable declared outside the loop**

I think the code here is correct because the variable is first defined outside the loop as being `None`.

https://github.com/indico/indico/blob/a22e0c0a9c0cf4d40d04029489dc530b4819d2ea/bin/utils/storage_checksums.py#L39-L43

https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/common.py#L581-L599

(many more)

An easy fix here could be to skip the rule if a binding for the same name was already defined before the for loop. But it may also be intentional that we catch this as part of this rule. What do you think?

**Use in combination with try..except**

It seems to be a somewhat common pattern to use the for loop variable outside the for-block when combined with `try..except`

https://github.com/python-trio/trio/blob/c9baca6045d427fd4cc0de2ca396ddeb4a0f40a5/src/trio/_tests/check_type_completeness.py#L62-L109

**Multiple usages in the same statement**

It would be great if we only flagged each variable once (at least per statement?)

+ [src/_pytest/assertion/rewrite.py:492:40:](https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492) RUF032 `for` loop variable i is used outside of block
+ [src/_pytest/assertion/rewrite.py:492:53:](https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492) RUF032 `for` loop variable i is used outside of block
+ [src/_pytest/assertion/rewrite.py:492:66:](https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492) RUF032 `for` loop variable i is used outside of block


**Delete statements **
This is interesting. We may explicitly want to allow usages in delete statements

https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/__init__.py#L19

**Control flow**

This here seems like a clear false positive but detecting it would be hard because it requires some form of control flow analysis

https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/plotting/_matplotlib/boxplot.py#L520-L561
https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L266-L276
https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L476-L486

---

_@MichaReiser reviewed on 2024-08-09 08:17_

The code changes look good to me but there are some common patterns that the rule isn't handling well which makes me a bit wary of shipping the rule as is. @AlexWaygood any thoughts on the examples?

---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:48 on 2024-08-09 16:53_

#### Variable declared outside the loop

> But it may also be intentional that we catch this as part of this rule. What do you think?

I think it's intentional that we catch the [`indico` example](https://github.com/indico/indico/blob/a22e0c0a9c0cf4d40d04029489dc530b4819d2ea/bin/utils/storage_checksums.py#L39-L43) as part of the rule. We have an example in the test fixture file for this.

The other [example from `pandas`](https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/common.py#L581-L599) could be addressed with some fancy control flow analysis (see [other discussion](https://github.com/astral-sh/ruff/pull/11769/#discussion_r1711844404))

---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:49 on 2024-08-09 17:08_

#### Use in combination with `try..except`

> It seems to be a somewhat common pattern to use the for loop variable outside the for-block when combined with `try..except`

The [`trio` example](https://github.com/python-trio/trio/blob/c9baca6045d427fd4cc0de2ca396ddeb4a0f40a5/src/trio/_tests/check_type_completeness.py#L62-L109) really lends itself well to the convenience that the non-block scoping provides. I think this is probably best resolved by adding a `# noqa: RUF032` comment for the lint rule to explicitly mark the case as fine.

---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:49 on 2024-08-09 17:15_

#### Multiple usages in the same statement

> It would be great if we only flagged each variable once (at least per statement?)
> 
> * [src/_pytest/assertion/rewrite.py:492:40:](https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492) RUF032 `for` loop variable i is used outside of block
> * [src/_pytest/assertion/rewrite.py:492:53:](https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492) RUF032 `for` loop variable i is used outside of block
> * [src/_pytest/assertion/rewrite.py:492:66:](https://github.com/pytest-dev/pytest/blob/ef9b8f9d748b6f50eab5d43e32d93008f7880899/src/_pytest/assertion/rewrite.py#L492) RUF032 `for` loop variable i is used outside of block

Seems like a decent improvement. Do we do this sort of deduplication for other rules? For the violation `Diagnostic`, do we combine the `TextRange` to cover the whole area?

---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:49 on 2024-08-09 17:15_

#### Delete statements

> We may explicitly want to allow usages in delete statements ([example](https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/__init__.py#L19))

Seems harmless to allow :+1: 



---

_Review comment by @MadLittleMods on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:49 on 2024-08-09 17:22_

#### Control flow

> This here seems like a clear false positive but detecting it would be hard because it requires some form of control flow analysis

The [`pandas` example](https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/plotting/_matplotlib/boxplot.py#L520-L561) makes sense to allow if we can detect the context via control flow analysis. The [other `pandas` example](https://github.com/pandas-dev/pandas/blob/0cdc6a48302ba1592b8825868de403ff9b0ea2a5/pandas/io/common.py#L581-L599) makes sense for this as well since it has a `return` statement to guard it every leaking below.

But I think it's good that we catch [`bokeh`](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L266-L276) [examples](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L476-L486) as sketchy. I don't see how we could use control flow analysis to see that they're fine?

---

I'm assuming we don't want to actually dive into implementing the control flow analysis in this first iteration?


---

_@MadLittleMods reviewed on 2024-08-09 17:23_

Split the conversation from https://github.com/astral-sh/ruff/pull/11769/#issuecomment-2277408079 up so we can better discuss each issue and resolve.

---

_Label `needs-decision` added by @MichaReiser on 2024-08-15 16:16_

---

_@AlexWaygood reviewed on 2024-08-19 10:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:48 on 2024-08-19 10:50_

So, I think there are two different reasons why you want to avoid using a loop variable after the loop has finished:
1. The first reason is that the loop might be empty, which means that the variable is never assigned:
   ```py
   iterable = []
   for x in iterable:
       pass
   print(x)  # NameError
   ```
   Catching these feels like a "correctness" issue, that we should hopefully be able to do without very many false positives.
2. The second is cases where we can be sure that the variable is defined, but we still forbid this pattern, perhaps on grounds of readability/style (it might not be obvious to the reader that the variable is mutated; there might be clearer ways of writing the code), or because there's a chance that the author of the code might not be familiar with Python's scoping rules, and might not have intended to reassign the variable by using the loop:
   ```py
   x = 42
   for x in range(5):
        pass
   print(x)  # oops, now it's 4?? The author of the code expected it to still be 42
   ```
   Sometimes code like this _can_ indicate a bug, but it also might be working exactly as the author of the code intended; there's a lot of code out there that deliberately mutates a variable using a loop like this.

The conclusion I draw from this is that we in fact need two rules here:
1. One rule to catch instances where a loop variable is used after the loop and was not defined before the loop. This is almost always bad practice, as the loop variable might not be defined.
2. One more opinionated rule that catches instances where the loop variable is definitely defined, but the mutation of the variable via the loop might not be desired. This might be a bug and might not be, depending on how experienced the developer is with Python's semantics.

For now I think I'd implement the first rule; we can consider the second rule as a followup.

---

_@AlexWaygood reviewed on 2024-08-19 10:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/ruff/for_variable_used_after_block.py`:49 on 2024-08-19 10:51_

Usages in `del` statements are just as problematic as other usages, since they also result in `NameError`s if the variable is undefined:

```pycon
>>> iterable = []
>>> for item in iterable:
...     pass
... 
>>> del item
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'item' is not defined. Did you mean: 'iter'?
```

---
