```yaml
number: 9485
title: (🐞) Add the missing period in error message
type: pull_request
state: merged
author: KotlinIsland
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-double-period
created_at: 2024-01-12T04:33:50Z
updated_at: 2024-01-12T04:51:57Z
url: https://github.com/astral-sh/ruff/pull/9485
synced_at: 2026-01-10T22:57:09Z
```

# (🐞) Add the missing period in error message

---

_Pull request opened by @KotlinIsland on 2024-01-12 04:33_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
I noticed that there should be a missing period added to some of the new error messages for Unnecessary dunder call:
```
sandpit\test.py:6:16: PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
```
## Test Plan

Static analysis of the implementation, as this has no existing test cases.


---

_Comment by @codspeed-hq[bot] on 2024-01-12 04:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/KotlinIsland:fix-double-period)

### Merging #9485 will **improve performances by 4.55%**

<sub>Comparing <code>KotlinIsland:fix-double-period</code> (330df7d) with <code>main</code> (a31a314)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `KotlinIsland:fix-double-period` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---

_@charliermarsh approved on 2024-01-12 04:43_

Thank you!

---

_@charliermarsh reviewed on 2024-01-12 04:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:294 on 2024-01-12 04:43_

I'm guessing that I did a search and replace here to remove the periods but missed all the variants that are on their own line :)

---

_Label `bug` added by @charliermarsh on 2024-01-12 04:43_

---

_Merged by @charliermarsh on 2024-01-12 04:43_

---

_Closed by @charliermarsh on 2024-01-12 04:43_

---

_Comment by @github-actions[bot] on 2024-01-12 04:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+200 -200 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+28 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/cli/commands/dag_command.py#L471'>airflow/cli/commands/dag_command.py:471:25:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/cli/commands/dag_command.py#L471'>airflow/cli/commands/dag_command.py:471:25:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/fab/auth_manager/cli_commands/user_command.py#L167'>airflow/providers/fab/auth_manager/cli_commands/user_command.py:167:44:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/fab/auth_manager/cli_commands/user_command.py#L167'>airflow/providers/fab/auth_manager/cli_commands/user_command.py:167:44:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/fab/auth_manager/cli_commands/user_command.py#L60'>airflow/providers/fab/auth_manager/cli_commands/user_command.py:60:66:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/fab/auth_manager/cli_commands/user_command.py#L60'>airflow/providers/fab/auth_manager/cli_commands/user_command.py:60:66:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/salesforce/hooks/salesforce.py#L194'>airflow/providers/salesforce/hooks/salesforce.py:194:16:</a> PLC2801 Unnecessary dunder call to `__getattr__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/salesforce/hooks/salesforce.py#L194'>airflow/providers/salesforce/hooks/salesforce.py:194:16:</a> PLC2801 Unnecessary dunder call to `__getattr__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/salesforce/operators/bulk.py#L102'>airflow/providers/salesforce/operators/bulk.py:102:22:</a> PLC2801 Unnecessary dunder call to `__getattr__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/apache/airflow/blob/83a3cc5e19b543f0e7c95d693f5315457fc0a910/airflow/providers/salesforce/operators/bulk.py#L102'>airflow/providers/salesforce/operators/bulk.py:102:22:</a> PLC2801 Unnecessary dunder call to `__getattr__`. Access attribute directly or use getattr built-in function..
... 46 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1164'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1164:24:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1164'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1164:24:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1165'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1165:35:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1165'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1165:35:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1192'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1192:24:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1192'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1192:24:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1193'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1193:35:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L1193'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:1193:35:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L856'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:856:24:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/aws/aws-sam-cli/blob/0e87d84f2b5e55c120cd191fe1074a9ea9d24deb/tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py#L856'>tests/unit/hook_packages/terraform/hooks/prepare/test_enrich.py:856:24:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L909'>src/bokeh/models/plots.py:909:20:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L909'>src/bokeh/models/plots.py:909:20:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+19 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L198'>ibis/common/bases.py:198:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L198'>ibis/common/bases.py:198:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L212'>ibis/common/bases.py:212:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L212'>ibis/common/bases.py:212:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L239'>ibis/common/bases.py:239:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L239'>ibis/common/bases.py:239:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L241'>ibis/common/bases.py:241:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L241'>ibis/common/bases.py:241:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L245'>ibis/common/bases.py:245:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/38aaf8fe17a50e2fd8dc541c2cc0c64522eea1aa/ibis/common/bases.py#L245'>ibis/common/bases.py:245:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+45 -45 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L227'>pandas/_config/config.py:227:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L227'>pandas/_config/config.py:227:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L228'>pandas/_config/config.py:228:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L228'>pandas/_config/config.py:228:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L231'>pandas/_config/config.py:231:18:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L231'>pandas/_config/config.py:231:18:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L243'>pandas/_config/config.py:243:18:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L243'>pandas/_config/config.py:243:18:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L248'>pandas/_config/config.py:248:17:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/_config/config.py#L248'>pandas/_config/config.py:248:17:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/pandas-dev/pandas/blob/5789f15402a97bbaf590c8de2696ef94c22a6bf9/pandas/core/accessor.py#L229'>pandas/core/accessor.py:229:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
... 79 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+99 -99 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L105'>rotkehlchen/assets/asset.py:105:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L105'>rotkehlchen/assets/asset.py:105:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L301'>rotkehlchen/assets/asset.py:301:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L301'>rotkehlchen/assets/asset.py:301:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L302'>rotkehlchen/assets/asset.py:302:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L302'>rotkehlchen/assets/asset.py:302:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L303'>rotkehlchen/assets/asset.py:303:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L303'>rotkehlchen/assets/asset.py:303:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L304'>rotkehlchen/assets/asset.py:304:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L304'>rotkehlchen/assets/asset.py:304:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L305'>rotkehlchen/assets/asset.py:305:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L305'>rotkehlchen/assets/asset.py:305:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L306'>rotkehlchen/assets/asset.py:306:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L306'>rotkehlchen/assets/asset.py:306:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L318'>rotkehlchen/assets/asset.py:318:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L318'>rotkehlchen/assets/asset.py:318:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L319'>rotkehlchen/assets/asset.py:319:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L319'>rotkehlchen/assets/asset.py:319:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L320'>rotkehlchen/assets/asset.py:320:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L320'>rotkehlchen/assets/asset.py:320:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L321'>rotkehlchen/assets/asset.py:321:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L321'>rotkehlchen/assets/asset.py:321:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L322'>rotkehlchen/assets/asset.py:322:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/rotki/rotki/blob/0d95c2d47f2b1a1d55dcaec30c8d1bc69e12c132/rotkehlchen/assets/asset.py#L322'>rotkehlchen/assets/asset.py:322:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
... 174 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2801 | 400 | 200 | 200 | 0 | 0 |

</p>
</details>




---

_Branch deleted on 2024-01-12 04:51_

---
