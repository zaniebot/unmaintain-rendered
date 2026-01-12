```yaml
number: 9440
title: "Add rule to enforce parentheses in `a or b and c`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: force-parens-lint
created_at: 2024-01-08T15:22:45Z
updated_at: 2024-01-11T11:42:25Z
url: https://github.com/astral-sh/ruff/pull/9440
synced_at: 2026-01-12T15:55:28Z
```

# Add rule to enforce parentheses in `a or b and c`

---

_@AlexWaygood_

Fixes #8721

## Summary

This implements the rule proposed in #8721, as RUF021. `and` always binds more tightly than `or` when chaining the two together.

(This should definitely be autofixable, but I'm leaving that to a followup PR for now.)

## Test Plan

`cargo test` / `cargo insta review`

---

_Comment by @github-actions[bot] on 2024-01-08 15:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+93 -0 violations, +0 -0 fixes in 11 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/converter.py#L1168'>disnake/ext/commands/converter.py:1168:12:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/interactions/base.py#L1880'>disnake/interactions/base.py:1880:21:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/interactions/base.py#L1905'>disnake/interactions/base.py:1905:21:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/interactions/base.py#L1923'>disnake/interactions/base.py:1923:18:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/interactions/base.py#L213'>disnake/interactions/base.py:213:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/cli/utils.py#L74'>rasa/cli/utils.py:74:27:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/processor.py#L587'>rasa/core/processor.py:587:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/core/training_data/structures.py#L619'>rasa/shared/core/training_data/structures.py:619:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/core/training_data/structures.py#L621'>rasa/shared/core/training_data/structures.py:621:20:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/train_utils.py#L534'>rasa/utils/train_utils.py:534:9:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/utils/train_utils.py#L536'>rasa/utils/train_utils.py:536:12:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/metrics/validators.py#L217'>airflow/metrics/validators.py:217:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/models/dag.py#L3222'>airflow/models/dag.py:3222:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/models/dagrun.py#L1070'>airflow/models/dagrun.py:1070:20:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/operators/python.py#L369'>airflow/operators/python.py:369:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/providers/cncf/kubernetes/operators/pod.py#L389'>airflow/providers/cncf/kubernetes/operators/pod.py:389:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/providers/oracle/hooks/oracle.py#L298'>airflow/providers/oracle/hooks/oracle.py:298:38:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/serialization/serde.py#L220'>airflow/serialization/serde.py:220:8:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/www/utils.py#L827'>airflow/www/utils.py:827:20:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/www/utils.py#L840'>airflow/www/utils.py:840:20:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/apache/airflow/blob/9264a4b4e21702a2bc71bb77ee3cc4ada9dfd5e7/airflow/www/validators.py#L50'>airflow/www/validators.py:50:32:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/dataspec.py#L334'>src/bokeh/core/property/dataspec.py:334:35:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/plotting/_graph.py#L74'>src/bokeh/plotting/_graph.py:74:8:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/a46bd4a15eb6bf746541cbb73d40aff18384230b/ibis/expr/analysis.py#L339'>ibis/expr/analysis.py:339:20:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/ibis-project/ibis/blob/a46bd4a15eb6bf746541cbb73d40aff18384230b/ibis/expr/datatypes/cast.py#L122'>ibis/expr/datatypes/cast.py:122:9:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/ibis-project/ibis/blob/a46bd4a15eb6bf746541cbb73d40aff18384230b/ibis/expr/datatypes/cast.py#L124'>ibis/expr/datatypes/cast.py:124:12:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/ibis-project/ibis/blob/a46bd4a15eb6bf746541cbb73d40aff18384230b/ibis/expr/datatypes/cast.py#L127'>ibis/expr/datatypes/cast.py:127:12:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/f3933024f8860ebd10e2447c62c5c9dc65e1d9dd/pymilvus/client/check.py#L238'>pymilvus/client/check.py:238:26:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/asv_bench/benchmarks/groupby.py#L514'>asv_bench/benchmarks/groupby.py:514:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/_testing/asserters.py#L754'>pandas/_testing/asserters.py:754:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/_testing/asserters.py#L756'>pandas/_testing/asserters.py:756:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/_testing/asserters.py#L911'>pandas/_testing/asserters.py:911:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/_testing/asserters.py#L913'>pandas/_testing/asserters.py:913:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/arrays/arrow/array.py#L1407'>pandas/core/arrays/arrow/array.py:1407:20:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/computation/eval.py#L340'>pandas/core/computation/eval.py:340:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/computation/expr.py#L510'>pandas/core/computation/expr.py:510:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/computation/pytables.py#L410'>pandas/core/computation/pytables.py:410:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/computation/pytables.py#L412'>pandas/core/computation/pytables.py:412:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/dtypes/dtypes.py#L1061'>pandas/core/dtypes/dtypes.py:1061:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/dtypes/dtypes.py#L1714'>pandas/core/dtypes/dtypes.py:1714:21:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/frame.py#L4035'>pandas/core/frame.py:4035:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/frame.py#L6894'>pandas/core/frame.py:6894:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/indexes/range.py#L1042'>pandas/core/indexes/range.py:1042:34:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/internals/construction.py#L493'>pandas/core/internals/construction.py:493:24:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/pandas-dev/pandas/blob/8cb60d0e229c1e2ca3661961e98e3365eb65336d/pandas/core/resample.py#L2126'>pandas/core/resample.py:2126:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/7497e457592336cc9f4f7ec2833225916b8b43d1/src/poetry/console/io/inputs/run_argv_input.py#L48'>src/poetry/console/io/inputs/run_argv_input.py:48:38:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/python-poetry/poetry/blob/7497e457592336cc9f4f7ec2833225916b8b43d1/src/poetry/utils/dependency_specification.py#L161'>src/poetry/utils/dependency_specification.py:161:16:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/python-poetry/poetry/blob/7497e457592336cc9f4f7ec2833225916b8b43d1/src/poetry/utils/env/env_manager.py#L235'>src/poetry/utils/env/env_manager.py:235:33:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/accounting/export/csv.py#L155'>rotkehlchen/accounting/export/csv.py:155:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/accounting/export/csv.py#L156'>rotkehlchen/accounting/export/csv.py:156:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/api/rest.py#L1076'>rotkehlchen/api/rest.py:1076:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/api/rest.py#L1077'>rotkehlchen/api/rest.py:1077:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/api/v1/schemas.py#L3223'>rotkehlchen/api/v1/schemas.py:3223:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/api/v1/schemas.py#L3224'>rotkehlchen/api/v1/schemas.py:3224:13:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/assets/utils.py#L261'>rotkehlchen/assets/utils.py:261:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/chain/ethereum/modules/uniswap/v2/common.py#L134'>rotkehlchen/chain/ethereum/modules/uniswap/v2/common.py:134:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L153'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:153:9:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/rotki/rotki/blob/b106564d79ab9a6407df783857bac262fbdbdbb7/rotkehlchen/chain/evm/decoding/metamask/decoder.py#L75'>rotkehlchen/chain/evm/decoding/metamask/decoder.py:75:17:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f8c5798897852e107df0683893de59e15c558baf/tests/test_settings_overrides.py#L401'>tests/test_settings_overrides.py:401:45:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/37c1b8891702716ae13e89c51aa548b54a547020/tools/lib/provision.py#L163'>tools/lib/provision.py:163:4:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/zulip/zulip/blob/37c1b8891702716ae13e89c51aa548b54a547020/tools/lib/provision.py#L163'>tools/lib/provision.py:163:51:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/zulip/zulip/blob/37c1b8891702716ae13e89c51aa548b54a547020/tools/lib/template_parser.py#L677'>tools/lib/template_parser.py:677:52:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/zulip/zulip/blob/37c1b8891702716ae13e89c51aa548b54a547020/zerver/lib/cache.py#L656'>zerver/lib/cache.py:656:12:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/zulip/zulip/blob/37c1b8891702716ae13e89c51aa548b54a547020/zerver/models/users.py#L1049'>zerver/models/users.py:1049:9:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
+ <a href='https://github.com/zulip/zulip/blob/37c1b8891702716ae13e89c51aa548b54a547020/zerver/models/users.py#L1051'>zerver/models/users.py:1051:12:</a> RUF021 Parenthesize `a and b` expressions when chaining `and` and `or` together, to make the precedence clear
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF021 | 93 | 93 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-01-08 15:44_

> `ruff-ecosystem` results

I found 45 errors on the mypy codebase when running ruff manually with this branch, interestingly (I guess mypy isn't one of the repos in the `ruff ecosystem` report at the moment). That's much more than any of the ones in the report here. Maybe AST munging in Python is particularly likely to end up with long comparison chains? 🤷

---

_Comment by @AlexWaygood on 2024-01-08 15:59_

I did a bit of manual testing with some of the mypy hits I found from running ruff manually with this branch. In particular, this span triggers six instances(!) of the new warning: https://github.com/python/mypy/blob/35f402c74205d373b81076ea82f507eb85161a25/mypy/messages.py#L2940-L2955

Applying this diff to mypy fixes all of them, while maintaining an identical Python AST. (And, neither `ruff format` nor `Black` would strip away the new parentheses.)

<details>

```diff
diff --git a/mypy/messages.py b/mypy/messages.py
index 450faf4c1..7dee269b1 100644
--- a/mypy/messages.py
+++ b/mypy/messages.py
@@ -2938,20 +2938,16 @@ def get_bad_protocol_flags(
     bad_flags = []
     for name, subflags, superflags in all_flags:
         if (
-            IS_CLASSVAR in subflags
-            and IS_CLASSVAR not in superflags
-            and IS_SETTABLE in superflags
-            or IS_CLASSVAR in superflags
-            and IS_CLASSVAR not in subflags
-            or IS_SETTABLE in superflags
-            and IS_SETTABLE not in subflags
-            or IS_CLASS_OR_STATIC in superflags
-            and IS_CLASS_OR_STATIC not in subflags
-            or class_obj
-            and IS_VAR in superflags
-            and IS_CLASSVAR not in subflags
-            or class_obj
-            and IS_CLASSVAR in superflags
+            (
+                IS_CLASSVAR in subflags
+                and IS_CLASSVAR not in superflags
+                and IS_SETTABLE in superflags
+            )
+            or (IS_CLASSVAR in superflags and IS_CLASSVAR not in subflags)
+            or (IS_SETTABLE in superflags and IS_SETTABLE not in subflags)
+            or (IS_CLASS_OR_STATIC in superflags and IS_CLASS_OR_STATIC not in subflags)
+            or (class_obj and IS_VAR in superflags and IS_CLASSVAR not in subflags)
+            or (class_obj and IS_CLASSVAR in superflags)
         ):
```

---

_Comment by @AlexWaygood on 2024-01-08 16:14_

I skimmed through most of the ecosystem results; I can't _see_ any false positives.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:34 on 2024-01-08 17:26_

I think you're missing a trailing ```` ``` ```` here? (Although CommonMark will insert an implicit closing ```` ``` ```` for you.)

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 17:34_

Do you need to explicitly match `BoolOp::And` here? Thinking about this, I _think_ you could also match `BoolOp::Or` here and the behavior would be unchanged? A quick comment noting this might be useful because the presence of `And` and not `Or` confused me for a moment. :-)

---

_@BurntSushi approved on 2024-01-08 17:34_

---

_Comment by @BurntSushi on 2024-01-08 17:40_

Looking through the ecosystem results, I see a few instances of the `x and y or z` idiom flagged. You also commonly see it used as `x and y or None`. I wonder if we want to flag those? AIUI, it's a somewhat common tactic for a ternary operator in Python, although it [has some gotchas](https://stackoverflow.com/questions/39080416/x-and-y-or-z-ternary-operator).

---

_@AlexWaygood reviewed on 2024-01-08 17:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 17:51_

Yeah, I don't _think_ we _need_ to explicitly match `BoolOp::And` here:
- An `or` chained with an `or` (or an `and` chained with an `and`) will just result in a `BoolOp` with three items in the `values` list, since the two operators have the same precedence
- An `and` chained with an `or` (or an `or` chained with an `and`) will both result in an `And` `BoolOp` inside an `Or` `BoolOp`, because the AST reflects the fact that `and` always binds more tightly than `or`.

Why did I leave this the way I did? Two reasons:
1. I got scared I was incorrect in my reasoning ^above, and it seemed "safer" to explicitly check here that it _was_ an `And`, for some definition of the word "safe" :) I could maybe turn it into an assertion instead, to check the invariant holds true -- WDYT?
2. It seemed _slightly_ more readable this way, as now it's obvious to the reader of the code that the subexpression is an `And` `BoolOp`. But that could also be done equally well with an assertion, I suppose!

---

_@AlexWaygood reviewed on 2024-01-08 17:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:34 on 2024-01-08 17:52_

oops, good catch 😶

---

_Comment by @AlexWaygood on 2024-01-08 18:02_

> Looking through the ecosystem results, I see a few instances of the `x and y or z` idiom flagged. You also commonly see it used as `x and y or None`. I wonder if we want to flag those? AIUI, it's a somewhat common tactic for a ternary operator in Python, although it [has some gotchas](https://stackoverflow.com/questions/39080416/x-and-y-or-z-ternary-operator).

Hmmm... I know it's reasonably common, especially in older codebases, but I'd honestly consider that something of an antipattern due to those gotchas. I'd definitely object to it if somebody tried submitting a CPython PR with it 😄

I suppose the precedence for `x and y or z` is less surprising than for `x or y and z`, so we _could_ exclude it for now. But I lean towards leaving it in -- codebases who find it too opinionated can presumably just not opt into this rule? WDYT?

---

_Comment by @codspeed-hq[bot] on 2024-01-08 18:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:force-parens-lint)

### Merging #9440 will **not alter performance**

<sub>Comparing <code>AlexWaygood:force-parens-lint</code> (ab36c83) with <code>main</code> (f419af4)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@charliermarsh reviewed on 2024-01-08 18:03_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:43 on 2024-01-08 18:03_

Nit can we say `A and B` or `a and b`, instead of `foo` and `bar`?



---

_@charliermarsh reviewed on 2024-01-08 18:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:49 on 2024-01-08 18:04_

Nit: as a kinda hacky thing, we typically add a rustdoc like `/// RUF021` to make it easy to jump to a rule definition from a code.

---

_@charliermarsh reviewed on 2024-01-08 18:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:67 on 2024-01-08 18:04_

Does `expr.into()` work here, or `bool_op.into()` above?

---

_@charliermarsh reviewed on 2024-01-08 18:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 18:04_

Nit: I think you can omit the `{}` here.

---

_@charliermarsh reviewed on 2024-01-08 18:05_

We could probably add an (unsafe) fix for this, if you're interested, by inserting parentheses around the expression.

---

_Comment by @AlexWaygood on 2024-01-08 18:06_

> We could probably add an (unsafe) fix for this, if you're interested, by inserting parentheses around the expression.

Yes, definitely! Would you prefer me to do that as part of this PR, or as a followup?

---

_@BurntSushi reviewed on 2024-01-08 18:08_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 18:08_

Oh I see now. You don't need to match on the operator at all. Just the existence of a nested `BoolOp` is enough. Right?

> because the AST reflects the fact that and always binds more tightly than or

Ah I didn't realize that.

I think I would go with an `assert!` here. To me, the reasoning is that if your _assumption_ here about the structure of the AST is wrong in some way, then it's likely this code has a bug, right? If so, IMO, it would be better to make that bug loud than to silently continue. i.e., "this tripped a panic" is easier to diagnose as errant behavior than "this lint is producing false negatives." The latter might not be noticed at all. A short comment on the `assert!` explaining this would be helpful IMO.

---

_Comment by @BurntSushi on 2024-01-08 18:10_

> > Looking through the ecosystem results, I see a few instances of the `x and y or z` idiom flagged. You also commonly see it used as `x and y or None`. I wonder if we want to flag those? AIUI, it's a somewhat common tactic for a ternary operator in Python, although it [has some gotchas](https://stackoverflow.com/questions/39080416/x-and-y-or-z-ternary-operator).
> 
> Hmmm... I know it's reasonably common, especially in older codebases, but I'd honestly consider that something of an antipattern due to those gotchas. I'd definitely object to it if somebody tried submitting a CPython PR with it 😄
> 
> I suppose the precedence for `x and y or z` is less surprising than for `x or y and z`, so we _could_ exclude it for now. But I lean towards leaving it in -- codebases who find it too opinionated can presumably just not opt into this rule? WDYT?

I don't think I have a strong opinion here honestly.

---

_@AlexWaygood reviewed on 2024-01-08 18:10_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 18:10_

> Oh I see now. You don't need to match on the operator at all. Just the existence of a nested `BoolOp` is enough. Right?

Precisely!

> I think I would go with an `assert!` here. To me, the reasoning is that if your _assumption_ here about the structure of the AST is wrong in some way, then it's likely this code has a bug, right? If so, IMO, it would be better to make that bug loud than to silently continue. i.e., "this tripped a panic" is easier to diagnose as errant behavior than "this lint is producing false negatives." The latter might not be noticed at all.

Okay, that all makes sense. I'll make the change -- 🤞 I'm right.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:67 on 2024-01-08 18:13_

yes

---

_@AlexWaygood reviewed on 2024-01-08 18:13_

---

_Comment by @AlexWaygood on 2024-01-08 18:33_

> Merging #9440 will **degrade performances by 5.36%**

That seems suboptimal... Are there any things I'm doing here that are obviously bad, performance-wise? Is this just because there are loads of `BoolOp` nodes in Python in general, and we have to visit all of them if this rule is enabled?

---

_Comment by @charliermarsh on 2024-01-08 18:41_

@AlexWaygood - Can you try rebasing or merging in `main`? We upgraded the Rust version, so my guess is that it's just comparing against the "wrong" baseline.

---

_@AlexWaygood reviewed on 2024-01-08 18:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:43 on 2024-01-08 18:49_

Thoughts on the error message here? I was torn between the current one and this, which is more verbose but maybe easier to understand:

```suggestion
            "Parenthesize `a and b` expressions when chaining `and` and `or` together, to make it clear that `and` binds more tightly than `or`"
```

---

_Comment by @charliermarsh on 2024-01-08 18:52_

> Yes, definitely! Would you prefer me to do that as part of this PR, or as a followup?

Separate PR is probably a better practice, since it'll enable this one to get merged sooner, but it's marginal :)

---

_Comment by @JelleZijlstra on 2024-01-08 18:53_

> Hmmm... I know it's reasonably common, especially in older codebases, but I'd honestly consider that something of an antipattern due to those gotchas.

I think it's mostly a legacy pattern from before the if-else ternary was added to Python (which was [Python 2.5](https://peps.python.org/pep-0308/)). I'd be OK with having a linter flag this pattern.

---

_@charliermarsh reviewed on 2024-01-08 18:53_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:43 on 2024-01-08 18:53_

I have a minor pref for the version you already have here. We tend towards having shorter messages, and put motivation in the documentation... In the future, we'll also include links to the documentation in the linter output.

---

_@JelleZijlstra reviewed on 2024-01-08 18:54_

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:43 on 2024-01-08 18:54_

Agree with the shorter one too. I would expect most programmers to know what "precedence" means.

---

_@AlexWaygood reviewed on 2024-01-08 18:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:43 on 2024-01-08 18:55_

Yeah, I also much prefer to keep things concise where possible. I'll keep this as it is then, thanks.

> In the future, we'll also include links to the documentation in the linter output.

That sounds awesome!

---

_Comment by @AlexWaygood on 2024-01-08 18:56_

> @AlexWaygood - Can you try rebasing or merging in `main`? We upgraded the Rust version, so my guess is that it's just comparing against the "wrong" baseline.

Looks like that was the issue, thanks 😅 By the way, do you have a preference between rebases or merge commits?

> Separate PR is probably a better practice, since it'll enable this one to get merged sooner, but it's marginal :)

Expect a followup PR tomorrow :D

---

_Comment by @charliermarsh on 2024-01-08 18:57_

@AlexWaygood - Within a PR, it's dealer's choice. But we always squash-and-merge (it's the only option enabled) when merging into `main`, such that every PR corresponds to a single commit. I personally tend to rebase my PRs.

---

_@AlexWaygood reviewed on 2024-01-08 22:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 22:29_

Well, the assertion fails on a bunch of stuff in the ecosystem checks, so I was obviously wrong about something somewhere 🙃

---

_Converted to draft by @AlexWaygood on 2024-01-08 22:29_

---

_@AlexWaygood reviewed on 2024-01-08 22:57_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 22:57_

Right, of course. It's very possible to have an `Or` inside an `And` -- but it only happens if the `or` is explicitly parenthesized (_otherwise_ the `and` binds more tightly, but the parentheses override that). I.e.:

```py
x = a and (b or c)
```

---

_Marked ready for review by @AlexWaygood on 2024-01-08 23:28_

---

_@AlexWaygood reviewed on 2024-01-08 23:29_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/parenthesize_logical_operators.rs`:74 on 2024-01-08 23:29_

fixed the crash in ab36c8382c4fd663db5f0c1c70b74a25c8fcc81f, and added some more comments :)

---

_Comment by @AlexWaygood on 2024-01-08 23:44_

Fixed the crash I accidentally introduced in 87ddff64a1934a5a0c9c00038551195315da1dd3. The ecosystem check is looking sane again now.

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-01-08 23:44_

---

_@charliermarsh approved on 2024-01-09 00:38_

---

_Label `rule` added by @charliermarsh on 2024-01-09 01:27_

---

_Label `preview` added by @charliermarsh on 2024-01-09 01:27_

---

_Merged by @charliermarsh on 2024-01-09 01:28_

---

_Closed by @charliermarsh on 2024-01-09 01:28_

---

_Comment by @charliermarsh on 2024-01-09 01:28_

Very nice!

---

_Branch deleted on 2024-01-09 07:01_

---

_Comment by @konstin on 2024-01-11 11:42_

Re `x and y or z`, we have a preview rule, [and-or-ternary (PLR1706)](https://docs.astral.sh/ruff/rules/and-or-ternary/), linting and fixing that case.

---
