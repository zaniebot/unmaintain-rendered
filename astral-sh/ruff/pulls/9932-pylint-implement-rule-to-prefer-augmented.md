```yaml
number: 9932
title: "[`pylint`] Implement rule to prefer augmented assignment (`PLR6104`)"
type: pull_request
state: merged
author: lshi18
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: 8877-new-rule-prefer-in-place-operators
created_at: 2024-02-11T16:27:34Z
updated_at: 2024-04-12T03:08:42Z
url: https://github.com/astral-sh/ruff/pull/9932
synced_at: 2026-01-12T15:55:30Z
```

# [`pylint`] Implement rule to prefer augmented assignment (`PLR6104`)

---

_@lshi18_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Implement new rule: Prefer augmented assignment (#8877). It checks for the assignment statement with the form of `<expr> = <expr> <binary-operator> …` with a unsafe fix to use augmented assignment instead.

## Test Plan

<!-- How was it tested? -->
1. Snapshot test is included in the PR.
2. Manually test with playground.

---

_Comment by @github-actions[bot] on 2024-02-11 16:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+507 -1 violations, +0 -0 fixes in 12 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/d9bbcd295832eab0a2da6bf81e0ae00b9777308a/aiven/client/client.py#L159'>aiven/client/client.py:159:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+33 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/analysis/tests/test_fit_functions.py#L241'>plasmapy/analysis/tests/test_fit_functions.py:241:13:</a> PLR6104 Use `*=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/analysis/tests/test_fit_functions.py#L263'>plasmapy/analysis/tests/test_fit_functions.py:263:13:</a> PLR6104 Use `*=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/analysis/time_series/running_moments.py#L61'>plasmapy/analysis/time_series/running_moments.py:61:5:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L413'>plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:413:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L419'>plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:419:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L830'>plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:830:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 27 additional changes omitted for rule PLR6104
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/formulary/collisions/helio/collisional_analysis.py#L238'>plasmapy/formulary/collisions/helio/collisional_analysis.py:238:9:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/40e94e4c1c88154a1b499e081bde902747256467/plasmapy/formulary/collisions/helio/collisional_analysis.py#L238'>plasmapy/formulary/collisions/helio/collisional_analysis.py:238:9:</a> PLR0914 Too many local variables (17/15)
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+47 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/io/path.py#L252'>airflow/io/path.py:252:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/io/path.py#L254'>airflow/io/path.py:254:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py#L89'>airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py:89:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py#L90'>airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py:90:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/models/taskinstance.py#L1635'>airflow/models/taskinstance.py:1635:21:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/providers/amazon/aws/hooks/eks.py#L546'>airflow/providers/amazon/aws/hooks/eks.py:546:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/providers/amazon/aws/hooks/eks.py#L549'>airflow/providers/amazon/aws/hooks/eks.py:549:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/providers/apache/hive/hooks/hive.py#L346'>airflow/providers/apache/hive/hooks/hive.py:346:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/providers/apache/hive/hooks/hive.py#L348'>airflow/providers/apache/hive/hooks/hive.py:348:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/4a3caa2e35e8a61e49a8e00ab7d5148cd460797f/airflow/providers/cncf/kubernetes/kube_client.py#L90'>airflow/providers/cncf/kubernetes/kube_client.py:90:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 37 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+128 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/commands/_utils/table_print.py#L46'>samcli/commands/_utils/table_print.py:46:9:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/commands/init/interactive_event_bridge_flow.py#L174'>samcli/commands/init/interactive_event_bridge_flow.py:174:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/build/bundler.py#L178'>samcli/lib/build/bundler.py:178:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/deploy/deployer.py#L468'>samcli/lib/deploy/deployer.py:468:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/deploy/deployer.py#L814'>samcli/lib/deploy/deployer.py:814:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/cli_paginator.py#L27'>samcli/lib/schemas/cli_paginator.py:27:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/cli_paginator.py#L34'>samcli/lib/schemas/cli_paginator.py:34:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/cli_paginator.py#L38'>samcli/lib/schemas/cli_paginator.py:38:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/cli_paginator.py#L42'>samcli/lib/schemas/cli_paginator.py:42:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/cli_paginator.py#L59'>samcli/lib/schemas/cli_paginator.py:59:9:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/schemas_aws_config.py#L52'>samcli/lib/schemas/schemas_aws_config.py:52:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/schemas/schemas_directory_hierarchy_builder.py#L14'>samcli/lib/schemas/schemas_directory_hierarchy_builder.py:14:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 116 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/server.py#L294'>src/bokeh/embed/server.py:294:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L379'>src/bokeh/resources.py:379:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L307'>src/bokeh/server/tornado.py:307:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L422'>src/bokeh/server/tornado.py:422:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/util.py#L96'>src/bokeh/server/util.py:96:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L308'>src/bokeh/util/token.py:308:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/pretty_bad_protocol/_util.py#L462'>securedrop/pretty_bad_protocol/_util.py:462:5:</a> PLR6104 Use `%=` to perform an augmented assignment directly
+ <a href='https://github.com/freedomofpress/securedrop/blob/6f7a725097fa02df01d3c8dd467a6faf40bdb3e7/securedrop/tests/test_journalist.py#L1864'>securedrop/tests/test_journalist.py:1864:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/backends/bigquery/tests/unit/udf/test_usage.py#L25'>ibis/backends/bigquery/tests/unit/udf/test_usage.py:25:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/backends/druid/__init__.py#L61'>ibis/backends/druid/__init__.py:61:9:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/backends/exasol/__init__.py#L100'>ibis/backends/exasol/__init__.py:100:9:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/backends/pandas/executor.py#L630'>ibis/backends/pandas/executor.py:630:17:</a> PLR6104 Use `&=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/common/annotations.py#L287'>ibis/common/annotations.py:287:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/common/annotations.py#L289'>ibis/common/annotations.py:289:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/common/tests/test_numeric.py#L63'>ibis/common/tests/test_numeric.py:63:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/common/tests/test_numeric.py#L68'>ibis/common/tests/test_numeric.py:68:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/common/tests/test_numeric.py#L75'>ibis/common/tests/test_numeric.py:75:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/cb906a6e9cf0278d29fda414e7143b0019059bde/ibis/expr/tests/test_api.py#L156'>ibis/expr/tests/test_api.py:156:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L142'>examples/example_bulkinsert_json.py:142:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L224'>examples/example_bulkinsert_json.py:224:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L247'>examples/example_bulkinsert_json.py:247:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L249'>examples/example_bulkinsert_json.py:249:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L251'>examples/example_bulkinsert_json.py:251:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L253'>examples/example_bulkinsert_json.py:253:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_json.py#L255'>examples/example_bulkinsert_json.py:255:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_numpy.py#L132'>examples/example_bulkinsert_numpy.py:132:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_numpy.py#L226'>examples/example_bulkinsert_numpy.py:226:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/e0f0e91e64f25991610d5b4f12caa941a7ee2748/examples/example_bulkinsert_numpy.py#L249'>examples/example_bulkinsert_numpy.py:249:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+131 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/asv_bench/benchmarks/algos/isin.py#L136'>asv_bench/benchmarks/algos/isin.py:136:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/asv_bench/benchmarks/arithmetic.py#L141'>asv_bench/benchmarks/arithmetic.py:141:13:</a> PLR6104 Use `//=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/_testing/contexts.py#L113'>pandas/_testing/contexts.py:113:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/compat/numpy/function.py#L107'>pandas/compat/numpy/function.py:107:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/compat/numpy/function.py#L168'>pandas/compat/numpy/function.py:168:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/compat/numpy/function.py#L200'>pandas/compat/numpy/function.py:200:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/compat/numpy/function.py#L229'>pandas/compat/numpy/function.py:229:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/algorithms.py#L900'>pandas/core/algorithms.py:900:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/apply.py#L1833'>pandas/core/apply.py:1833:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/arrow/array.py#L2544'>pandas/core/arrays/arrow/array.py:2544:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/boolean.py#L229'>pandas/core/arrays/boolean.py:229:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/4fe49b160eace2016955878a1c10ebacc5f885f3/pandas/core/arrays/boolean.py#L236'>pandas/core/arrays/boolean.py:236:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
... 119 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+45 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/package.py#L1061'>package.py:1061:17:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/package.py#L1063'>package.py:1063:17:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/package.py#L306'>package.py:306:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/package.py#L339'>package.py:339:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/rotkehlchen/chain/aggregator.py#L1313'>rotkehlchen/chain/aggregator.py:1313:17:</a> PLR6104 Use `*=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/rotkehlchen/chain/bitcoin/bch/utils.py#L132'>rotkehlchen/chain/bitcoin/bch/utils.py:132:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/rotkehlchen/chain/bitcoin/bch/utils.py#L58'>rotkehlchen/chain/bitcoin/bch/utils.py:58:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/rotkehlchen/chain/ethereum/graph.py#L71'>rotkehlchen/chain/ethereum/graph.py:71:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/rotkehlchen/chain/ethereum/modules/eth2/beacon.py#L76'>rotkehlchen/chain/ethereum/modules/eth2/beacon.py:76:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/rotki/rotki/blob/511ee939e11e6f1d1d645ce4c7e9a1ad16475d20/rotkehlchen/chain/ethereum/modules/makerdao/dsr.py#L377'>rotkehlchen/chain/ethereum/modules/makerdao/dsr.py:377:9:</a> PLR6104 Use `*=` to perform an augmented assignment directly
... 35 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6104 | 506 | 506 | 0 | 0 | 0 |
| PLR0914 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-02-11 17:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/lshi18:8877-new-rule-prefer-in-place-operators)

### Merging #9932 will **not alter performance**

<sub>Comparing <code>lshi18:8877-new-rule-prefer-in-place-operators</code> (5afd7de) with <code>main</code> (563daa8)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @Avasam on 2024-02-11 20:08_

Not a rust dev, so I won't comment on your implementation, but I think the tests should also check for proper handling of order of operation:
```python
>>> test = 2
>>> test = test * test + 10  # OK
>>> test
14
>>> test = 2
>>> test *= test + 10       
>>> test
24
>>> test = 2
>>> test = test * (test + 10)  # RUF028
>>> test     
24
```

---

_Comment by @lshi18 on 2024-02-11 20:57_

Thanks for the quick reply. I added the suggested test cases. I had these two test cases in my earlier draft, but wasn’t sure and removed them. It is nice that you brought them up and have them back. 

---

_Comment by @sbrugman on 2024-02-13 20:28_

Related pylint rule: 
https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-using-augmented-assign.html

Related issue: https://github.com/astral-sh/ruff/issues/970

@lshi18 instead of assigning it to RUF028, this rule should be placed under PyLint R6104. 

---

_Renamed from "Implement new rule: Prefer augmented assignment (#8877)" to "Implement new rule PLR6104: Prefer augmented assignment (#8877)" by @lshi18 on 2024-02-18 11:20_

---

_Comment by @lshi18 on 2024-02-18 11:22_

The new rule has been moved under PLR6104 instead of RUF028.

---

_Comment by @lshi18 on 2024-02-18 12:30_

The `CI/ecosystem` check failed with an error. I am not able to figure out what I did wrong immediately. It'll be of great help if you could point me to the right direction which could effect an efficient fix. 

---

_@sbrugman reviewed on 2024-02-18 14:33_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/binary_op_and_normal_assignment.rs`:70 on 2024-02-18 14:33_

Using [let-else](https://doc.rust-lang.org/beta/rust-by-example/flow_control/let_else.html) here to make this check a bit nicer:

```rust
let target = Some(targets.first()) else {
    return;
}
```

---

_@sbrugman reviewed on 2024-02-18 14:39_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/binary_op_and_normal_assignment.rs`:75 on 2024-02-18 14:39_

```rust
 let (left_operand, operator, right_operand) = Some(
      value
       .as_bin_op_expr()
       .map(|e| (e.left.as_ref(), e.op, e.right.as_ref()))
      ) else {
       return;
}
```

See [let](https://doc.rust-lang.org/std/keyword.let.html)

---

_@sbrugman reviewed on 2024-02-18 14:50_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/binary_op_and_normal_assignment.rs`:83 on 2024-02-18 14:50_

Instead of testing these conditions, you could match the target/left_operand type with match.

There may be some other rules to serve as inspiration e.g.
https://github.com/astral-sh/ruff/blob/235cfb79769da2c435b9c88d8bae4a79f1234857/crates/ruff_linter/src/rules/flake8_bugbear/rules/static_key_dict_comprehension.rs#L73

---

_@lshi18 reviewed on 2024-02-18 15:57_

---

_Review comment by @lshi18 on `crates/ruff_linter/src/rules/pylint/rules/binary_op_and_normal_assignment.rs`:70 on 2024-02-18 15:57_

Please correct me if I am wrong, but do you mean 

```rust
let Some(target) = target.first() else {
    return;
}
```

I agree that this looks nicer if it could be changed to it. 

I don't think, in this case, that they are semantically equal. In particular, the following test case which is marked as `OK` fails after the suggested change.

```python
a_number = index = a_number + 1 # OK
```

I think a further question would be whether we should detect the above case and, in the same gist, the one below.

```python
index = a_number = a_number + 1 # OK
```

---

_@sbrugman reviewed on 2024-02-18 17:34_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/binary_op_and_normal_assignment.rs`:70 on 2024-02-18 17:34_

My bad, I hadn't tested. Since targets is a vector, this should work:
```rust
let targets = vec![1];
// let targets = vec![1, 2];
// let targets: Vec<i32> = vec![];

let &[target] = targets.as_slice() else{
    return;
};
```

---

_@lshi18 reviewed on 2024-02-18 19:25_

---

_Review comment by @lshi18 on `crates/ruff_linter/src/rules/pylint/rules/binary_op_and_normal_assignment.rs`:70 on 2024-02-18 19:25_

I had a quick test with `pylint 3.0.3`, whose `R6104` rule detects this

```python
a_number = index = a_number + 1 # OK
```

but not this,
```python
index = a_number = a_number + 1 # OK
```

Do you think we should implement to reproduce this effect? 

BTW, [the test cases](https://github.com/pylint-dev/pylint/blob/2088414ef7353f98b65f61ff211150d9d7816c89/tests/functional/ext/code_style/cs_consider_using_augmented_assign.py#L84) for this rule in the pylint project include neither of the above cases, so I think the current behaviour is a side-effect of implementation. 

Meanwhile, my current implementation is incorrect regarding handling more complex subsript / attribute expressions. I'll reimplement it while also taking into consideration of [your other suggestions ](https://github.com/astral-sh/ruff/pull/9932#discussion_r1493782265).

---

_Renamed from "Implement new rule PLR6104: Prefer augmented assignment (#8877)" to "[`pylint`] Implement rule to prefer augmented assignment (`PLR6104`)" by @charliermarsh on 2024-04-12 02:43_

---

_Label `rule` added by @charliermarsh on 2024-04-12 02:43_

---

_Label `preview` added by @charliermarsh on 2024-04-12 02:43_

---

_@charliermarsh approved on 2024-04-12 02:44_

---

_Merged by @charliermarsh on 2024-04-12 03:08_

---

_Closed by @charliermarsh on 2024-04-12 03:08_

---
