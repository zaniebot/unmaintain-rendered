```yaml
number: 18256
title: Add Autofix for ISC003
type: pull_request
state: merged
author: maxmynter
labels:
  - fixes
assignees: []
merged: true
base: main
head: isc003
created_at: 2025-05-22T16:23:47Z
updated_at: 2025-05-28T07:30:52Z
url: https://github.com/astral-sh/ruff/pull/18256
synced_at: 2026-01-12T15:56:15Z
```

# Add Autofix for ISC003

---

_@maxmynter_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Closes: #18081 
## Summary
Add the following fix for `ICS003` (explicit string concatenation): Remove the `+` operator to implicitly concatenate {r,f,b}strings. 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Extended and adapted the `ICS.py` test fixture.
- Added tests for `b` and `rb` strings
- Updated the snapshot for existing tests to reflect the suggested fix

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-22 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +50 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +36 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/bigquery.py#L81'>superset/db_engine_specs/bigquery.py:81:5:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/bigquery.py#L81'>superset/db_engine_specs/bigquery.py:81:5:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/clickhouse.py#L292'>superset/db_engine_specs/clickhouse.py:292:17:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/clickhouse.py#L292'>superset/db_engine_specs/clickhouse.py:292:17:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L37'>superset/db_engine_specs/pinot.py:37:27:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L37'>superset/db_engine_specs/pinot.py:37:27:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L39'>superset/db_engine_specs/pinot.py:39:27:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L39'>superset/db_engine_specs/pinot.py:39:27:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L41'>superset/db_engine_specs/pinot.py:41:33:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L41'>superset/db_engine_specs/pinot.py:41:33:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L43'>superset/db_engine_specs/pinot.py:43:32:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L43'>superset/db_engine_specs/pinot.py:43:32:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L45'>superset/db_engine_specs/pinot.py:45:36:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L45'>superset/db_engine_specs/pinot.py:45:36:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L47'>superset/db_engine_specs/pinot.py:47:35:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L47'>superset/db_engine_specs/pinot.py:47:35:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L52'>superset/db_engine_specs/pinot.py:52:26:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L52'>superset/db_engine_specs/pinot.py:52:26:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L54'>superset/db_engine_specs/pinot.py:54:28:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L54'>superset/db_engine_specs/pinot.py:54:28:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L68'>superset/db_engine_specs/pinot.py:68:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L68'>superset/db_engine_specs/pinot.py:68:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1395'>superset/viz.py:1395:29:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1395'>superset/viz.py:1395:29:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1435'>superset/viz.py:1435:25:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1435'>superset/viz.py:1435:25:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L31'>tests/integration_tests/db_engine_specs/pinot_tests.py:31:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L31'>tests/integration_tests/db_engine_specs/pinot_tests.py:31:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L49'>tests/integration_tests/db_engine_specs/pinot_tests.py:49:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L49'>tests/integration_tests/db_engine_specs/pinot_tests.py:49:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L66'>tests/integration_tests/db_engine_specs/pinot_tests.py:66:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L66'>tests/integration_tests/db_engine_specs/pinot_tests.py:66:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L77'>tests/integration_tests/db_engine_specs/pinot_tests.py:77:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L77'>tests/integration_tests/db_engine_specs/pinot_tests.py:77:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/utils_tests.py#L352'>tests/integration_tests/utils_tests.py:352:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/utils_tests.py#L352'>tests/integration_tests/utils_tests.py:352:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_schrodinger.py#L26'>examples/styling/mathtext/latex_schrodinger.py:26:5:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_schrodinger.py#L26'>examples/styling/mathtext/latex_schrodinger.py:26:5:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L899'>src/bokeh/command/subcommands/serve.py:899:17:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L899'>src/bokeh/command/subcommands/serve.py:899:17:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L90'>src/bokeh/server/session.py:90:40:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L90'>src/bokeh/server/session.py:90:40:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L411'>src/bokeh/util/compiler.py:411:24:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L411'>src/bokeh/util/compiler.py:411:24:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/centromere/ctx.py#L282'>src/latch_cli/centromere/ctx.py:282:29:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/centromere/ctx.py#L282'>src/latch_cli/centromere/ctx.py:282:29:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L181'>src/latch_cli/utils/path.py:181:9:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L181'>src/latch_cli/utils/path.py:181:9:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L189'>src/latch_cli/utils/path.py:189:9:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L189'>src/latch_cli/utils/path.py:189:9:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ISC003 | 50 | 0 | 0 | 50 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +50 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +36 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/bigquery.py#L81'>superset/db_engine_specs/bigquery.py:81:5:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/bigquery.py#L81'>superset/db_engine_specs/bigquery.py:81:5:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/clickhouse.py#L292'>superset/db_engine_specs/clickhouse.py:292:17:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/clickhouse.py#L292'>superset/db_engine_specs/clickhouse.py:292:17:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L37'>superset/db_engine_specs/pinot.py:37:27:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L37'>superset/db_engine_specs/pinot.py:37:27:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L39'>superset/db_engine_specs/pinot.py:39:27:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L39'>superset/db_engine_specs/pinot.py:39:27:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L41'>superset/db_engine_specs/pinot.py:41:33:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L41'>superset/db_engine_specs/pinot.py:41:33:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L43'>superset/db_engine_specs/pinot.py:43:32:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L43'>superset/db_engine_specs/pinot.py:43:32:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L45'>superset/db_engine_specs/pinot.py:45:36:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L45'>superset/db_engine_specs/pinot.py:45:36:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L47'>superset/db_engine_specs/pinot.py:47:35:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L47'>superset/db_engine_specs/pinot.py:47:35:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L52'>superset/db_engine_specs/pinot.py:52:26:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L52'>superset/db_engine_specs/pinot.py:52:26:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L54'>superset/db_engine_specs/pinot.py:54:28:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L54'>superset/db_engine_specs/pinot.py:54:28:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L68'>superset/db_engine_specs/pinot.py:68:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/db_engine_specs/pinot.py#L68'>superset/db_engine_specs/pinot.py:68:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1395'>superset/viz.py:1395:29:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1395'>superset/viz.py:1395:29:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1435'>superset/viz.py:1435:25:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/superset/viz.py#L1435'>superset/viz.py:1435:25:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L31'>tests/integration_tests/db_engine_specs/pinot_tests.py:31:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L31'>tests/integration_tests/db_engine_specs/pinot_tests.py:31:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L49'>tests/integration_tests/db_engine_specs/pinot_tests.py:49:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L49'>tests/integration_tests/db_engine_specs/pinot_tests.py:49:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L66'>tests/integration_tests/db_engine_specs/pinot_tests.py:66:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L66'>tests/integration_tests/db_engine_specs/pinot_tests.py:66:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L77'>tests/integration_tests/db_engine_specs/pinot_tests.py:77:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/db_engine_specs/pinot_tests.py#L77'>tests/integration_tests/db_engine_specs/pinot_tests.py:77:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/utils_tests.py#L352'>tests/integration_tests/utils_tests.py:352:13:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/apache/superset/blob/0fa3feb08889f99a4cb8811dac81ec0a3b536da3/tests/integration_tests/utils_tests.py#L352'>tests/integration_tests/utils_tests.py:352:13:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_schrodinger.py#L26'>examples/styling/mathtext/latex_schrodinger.py:26:5:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_schrodinger.py#L26'>examples/styling/mathtext/latex_schrodinger.py:26:5:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L899'>src/bokeh/command/subcommands/serve.py:899:17:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L899'>src/bokeh/command/subcommands/serve.py:899:17:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L90'>src/bokeh/server/session.py:90:40:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L90'>src/bokeh/server/session.py:90:40:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L411'>src/bokeh/util/compiler.py:411:24:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L411'>src/bokeh/util/compiler.py:411:24:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/centromere/ctx.py#L282'>src/latch_cli/centromere/ctx.py:282:29:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/centromere/ctx.py#L282'>src/latch_cli/centromere/ctx.py:282:29:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L181'>src/latch_cli/utils/path.py:181:9:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L181'>src/latch_cli/utils/path.py:181:9:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
- <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L189'>src/latch_cli/utils/path.py:189:9:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
+ <a href='https://github.com/latchbio/latch/blob/e031e4c3c4b60e7820175c6e686d9a8910495c57/src/latch_cli/utils/path.py#L189'>src/latch_cli/utils/path.py:189:9:</a> ISC003 [*] Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ISC003 | 50 | 0 | 0 | 50 | 0 |

</p>
</details>




---

_Comment by @maxmynter on 2025-05-22 21:26_

The ecosystem changes all seem to be the change from a violation to a fixable violation.

Do we need a preview gate in this case? 

cc: @MichaReiser (because we were talking preview gates on an other PR and you've categorised the issue).  

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:41 on 2025-05-25 10:46_

Nit: You can implement `AlwaysFixableViolation` instead of specifying `FIX_AVAILABILITY = Always

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:81 on 2025-05-25 10:48_

Given that this uses `try_set_fix`, this would suggest that this fix isn't always available. 

This makes me wonder. Should, instead, the rule be changed that it doesn't flag cases where the left and right can't be implicitly concatenated?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:69 on 2025-05-25 10:49_

Unrelated to your PR but
```suggestion
            op: Operator::Add,
```

and you can then remove the match on line 71

We could also inline the match on line 72 and 77 by rewriting this to

```
left @ (Expr::FString(_) | Expr::StringLiteral(_) | Expr::BytesLiteral(_)
right @ (...)
```

and the same for right

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_implicit_str_concat/ISC.py`:94 on 2025-05-25 10:50_

Could you add an example where there are multiple adds: `"a" + "b" + "c" + "d" + "e"`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC003_ISC.py.snap`:20 on 2025-05-25 10:53_

Given that this is the motivation for the rule:

> For string literals that wrap across multiple lines, implicit string concatenation within parentheses is preferred over explicit concatenation using the + operator, as the former is more readable.

I think it's important that the fix preserves the fact that each literal is on its own line (or it produces ISC001). 

---

_@MichaReiser requested changes on 2025-05-25 10:54_

I think we need to change the fix to keep each part on its own line. We should also change the rule (separate PR) to only flag concatenations that can be converted to implicit string concatenations

---

_Label `fixes` added by @MichaReiser on 2025-05-25 10:54_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:69 on 2025-05-26 11:13_

I don't think the inlining of `left` and `right` works because they are `Box<Expr>` but the `box` destructuring is experimental, so we cannot do:

```rust 
if let ast::ExprBinOp {
    left: left @ box (Expr::FString(_) | Expr::StringLiteral(_) | Expr::BytesLiteral(_)),
    right: right @ box (Expr::FString(_) | Expr::StringLiteral(_) | Expr::BytesLiteral(_)),
    range,
    op: Operator::Add,
} = bin_op {...}
```

But we can definitely get rid of the `op` match. 

---

_@maxmynter reviewed on 2025-05-26 11:13_

---

_Comment by @maxmynter on 2025-05-26 12:47_

Thanks for the review. 

Changing `ISC003` to only fire when fixable and preserve formatting greatly reduces pattern match boilerplate in the fix. 
This also removes the Always / Sometimes fixable inconsistency. 

I've implemented the `AlwaysFixableViolation` and dropped the `Option` wrapping and `try_set_fix` handling.

I added mulitline test cases. The Diagnostic only warns for the first violation. But testing with the binary, fixes are applied recursively so the following is fully fixed when using ruff:
```bash
cat > test.py << 'EOF'
_ = ("a"
    + "b"
    + "c"
    + "d"
    + "e")
EOF
```

```bash
target/debug/ruff check --diff --select ISC003 --no-cache test.py
--- test.py
+++ test.py
@@ -1,5 +1,5 @@
 _ = ("a"
-    + "b"
-    + "c"
-    + "d"
-    + "e")
+     "b"
+     "c"
+     "d"
+     "e")

Would fix 4 errors.
```

---

_Review requested from @MichaReiser by @maxmynter on 2025-05-26 12:49_

---

_@MichaReiser reviewed on 2025-05-26 16:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC003_ISC.py.snap`:39 on 2025-05-26 16:33_

It seems that the fix failes to remove the trailing whitespace. Can you try to remove it too?

---

_@dscorbett reviewed on 2025-05-26 22:27_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:94 on 2025-05-26 22:27_

A newline can also be `'\r'` instead of `'\n'`.

---

_Review requested from @MichaReiser by @maxmynter on 2025-05-26 22:32_

---

_Review requested from @dscorbett by @maxmynter on 2025-05-26 22:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:86 on 2025-05-27 06:06_

```suggestion
    let between_operands_range = TextRange::new(left.end(), right.start());
    let between_operands = checker.locator().slice(between_operands_range);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:94 on 2025-05-27 06:08_

Can we add some more multiline tests, demonstrating that this works as expected. E.g. 

```py
_ = (
    "start" +
    variable + # comment
    "end"
)
```

```py
_ = (
    "start" +
    variable 
		# leading comment
    + "end"
)
```

I think what I would do instead of using `contains` is to call `trim_end_matches(' ')`. This also avoids trimming any linefeed characters.

---

_@MichaReiser reviewed on 2025-05-27 06:08_

---

_@dscorbett reviewed on 2025-05-27 11:15_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:94 on 2025-05-27 11:15_

`trim_end_matches(|c| c == '\t' || c == '\f' || c == ' ')` would catch all the non-newline spaces.

---

_@maxmynter reviewed on 2025-05-27 13:45_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:94 on 2025-05-27 13:45_

I found a function `is_python_whitespace` in `ruff_python_trivia` to accomplish the same. 

The check for the linebreak in `before` or `after` is necessary to check where we must trim. But I used the `checker.locator()` to handle all different line endings. 

Tests added in [ac7ab3c](https://github.com/astral-sh/ruff/pull/18256/commits/ac7ab3cd2dcc2b89dcce7fcd0831f613e749e35f)

---

_Comment by @maxmynter on 2025-05-27 14:02_

Note: Currently a case like this 

```python 
_ = (
    "start" +
    variable 
		# leading comment
    + "end"
    + "end"
)
```
Would not be reformatted. Since the first `+` is necessary and thus the rule is not recursively applied.

Likewise 
```python
_ = (
    "start" +
    "middle" + 
    variable 
    + "end"
    + "end"
)
```
will only fix the concatenation between `"start"` and `"middle"` not between `"end"` and `"end"`.

Is there another way to fix this other than to make the fix recurse through the whole string rather than rely on the recursive application of the rule?

I feel it would make this implementation a lot more involved. Which I'm happy to try, but wanted to double check. If so, are there other rules you'd recommend to look at as reference? 

---

_Review requested from @MichaReiser by @maxmynter on 2025-05-27 14:03_

---

_@MichaReiser approved on 2025-05-28 07:24_

---

_Comment by @MichaReiser on 2025-05-28 07:24_

Thank you

---

_Comment by @MichaReiser on 2025-05-28 07:28_

> Note: Currently a case like this
> 
> ```python
> _ = (
>     "start" +
>     variable 
> 		# leading comment
>     + "end"
>     + "end"
> )
> ```
> 
> Would not be reformatted. Since the first `+` is necessary and thus the rule is not recursively applied.
> 
> Likewise
> 
> ```python
> _ = (
>     "start" +
>     "middle" + 
>     variable 
>     + "end"
>     + "end"
> )
> ```
> 
> will only fix the concatenation between `"start"` and `"middle"` not between `"end"` and `"end"`.
> 
> Is there another way to fix this other than to make the fix recurse through the whole string rather than rely on the recursive application of the rule?
> 
> I feel it would make this implementation a lot more involved. Which I'm happy to try, but wanted to double-check. If so, are there other rules you'd recommend to look at as reference?

Given that this is a pre-existing limitation, I'd prefer to handle this in a separate PR. 

I think what we would need to do here is to "flatten-out" the binary expression for operators with the same precedence. We do this in the formatter but it can get a bit involved. But it requires you to recursively visit binary expressions and track what the last left side was (and if the operators have the same precedence and stop traversing if they don't)

---

_Merged by @MichaReiser on 2025-05-28 07:30_

---

_Closed by @MichaReiser on 2025-05-28 07:30_

---
