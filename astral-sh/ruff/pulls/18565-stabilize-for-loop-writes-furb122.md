```yaml
number: 18565
title: "Stabilize `for-loop-writes` (`FURB122`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-furb122
created_at: 2025-06-08T19:18:22Z
updated_at: 2025-06-09T20:10:20Z
url: https://github.com/astral-sh/ruff/pull/18565
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `for-loop-writes` (`FURB122`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:18_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:18_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:18_

---

_Comment by @github-actions[bot] on 2025-06-08 19:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+11 -1 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L147'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:147:21:</a> FURB122 [*] Use of `destination_file.write` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/devel-common/src/sphinx_exts/extra_files_with_substitutions.py#L41'>devel-common/src/sphinx_exts/extra_files_with_substitutions.py:41:17:</a> FURB122 [*] Use of `output_file.write` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L233'>providers/amazon/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:233:13:</a> FURB122 [*] Use of `temp_file.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/tests/integration_tests/core_tests.py#L386'>tests/integration_tests/core_tests.py:386:13:</a> FURB122 Use of `test_file.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L333'>scripts/milestone.py:333:9:</a> FURB122 [*] Use of `out.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/services/get_params.py#L177'>src/latch_cli/services/get_params.py:177:9:</a> FURB122 [*] Use of `f.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/tools/droplets/add_mentor.py#L69'>tools/droplets/add_mentor.py:69:13:</a> FURB122 [*] Use of `f.write` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/test_helpers.py#L574'>zerver/lib/test_helpers.py:574:13:</a> FURB122 [*] Use of `f.write` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/test_helpers.py#L784'>zerver/lib/test_helpers.py:784:5:</a> D400 First line should end with a period
- <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/test_helpers.py#L784'>zerver/lib/test_helpers.py:784:5:</a> D400 First line should end with a period
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/test_runner.py#L305'>zerver/lib/test_runner.py:305:17:</a> FURB122 [*] Use of `f.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/samp/lockfile_helpers.py#L46'>astropy/samp/lockfile_helpers.py:46:5:</a> FURB122 [*] Use of `lockfile.write` in a for loop
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB122 | 10 | 10 | 0 | 0 | 0 |
| D400 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:27_

---

_@ntBre approved on 2025-06-09 18:51_

---

_Merged by @dylwil3 on 2025-06-09 20:10_

---

_Closed by @dylwil3 on 2025-06-09 20:10_

---

_Branch deleted on 2025-06-09 20:10_

---
