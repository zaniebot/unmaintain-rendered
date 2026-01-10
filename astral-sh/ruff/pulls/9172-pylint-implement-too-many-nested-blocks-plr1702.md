```yaml
number: 9172
title: "[`pylint`] Implement `too-many-nested-blocks` (`PLR1702`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-R1702
created_at: 2023-12-17T21:55:11Z
updated_at: 2024-01-24T19:38:08Z
url: https://github.com/astral-sh/ruff/pull/9172
synced_at: 2026-01-10T22:57:09Z
```

# [`pylint`] Implement `too-many-nested-blocks` (`PLR1702`)

---

_Pull request opened by @diceroll123 on 2023-12-17 21:55_

## Summary

Implement [`PLR1702`/`too-many-nested-blocks`](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/too-many-nested-blocks.html)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @codspeed-hq[bot] on 2023-12-17 22:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-R1702)

### Merging #9172 will **not alter performance**

<sub>Comparing <code>diceroll123:add-R1702</code> (d18e76d) with <code>main</code> (fc3e266)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-12-17 22:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+95 -0 violations, +0 -0 fixes in 9 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/02765178014d5d9ded0eeec6e234606dab9f2c60/aiven/client/cli.py#L847'>aiven/client/cli.py:847:9:</a> PLR1702 Too many nested blocks (6 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/jobs/backfill_job_runner.py#L480'>airflow/jobs/backfill_job_runner.py:480:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L425'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:425:13:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L425'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:425:13:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/databricks/operators/databricks.py#L58'>airflow/providers/databricks/operators/databricks.py:58:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/apache/airflow/blob/390eacb01a710239e69f71d0c52134b7e87d17d4/airflow/providers/databricks/operators/databricks.py#L58'>airflow/providers/databricks/operators/databricks.py:58:5:</a> PLR1702 Too many nested blocks (7 > 5)
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/75862c906bc1d18e1d75ca323be6a17e28576e8f/samcli/commands/sync/command.py#L325'>samcli/commands/sync/command.py:325:5:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/75862c906bc1d18e1d75ca323be6a17e28576e8f/samcli/lib/package/utils.py#L262'>samcli/lib/package/utils.py:262:5:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/75862c906bc1d18e1d75ca323be6a17e28576e8f/samcli/lib/utils/subprocess_utils.py#L87'>samcli/lib/utils/subprocess_utils.py:87:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/75862c906bc1d18e1d75ca323be6a17e28576e8f/samcli/lib/utils/subprocess_utils.py#L87'>samcli/lib/utils/subprocess_utils.py:87:5:</a> PLR1702 Too many nested blocks (6 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/query.py#L178'>src/bokeh/core/query.py:178:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/query.py#L178'>src/bokeh/core/query.py:178:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/tests/unit/bokeh/command/subcommands/test_serve.py#L508'>tests/unit/bokeh/command/subcommands/test_serve.py:508:5:</a> PLR1702 Too many nested blocks (6 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/6d22d91be714bd4c41608bfe6c4918ccc6e79e67/admin/bootstrap.py#L85'>admin/bootstrap.py:85:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/6d22d91be714bd4c41608bfe6c4918ccc6e79e67/securedrop/pretty_bad_protocol/_parsers.py#L294'>securedrop/pretty_bad_protocol/_parsers.py:294:9:</a> PLR1702 Too many nested blocks (6 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/58174e5e19fb22ba48ad4722953fc88428292e11/ci/check_disallowed_imports.py#L24'>ci/check_disallowed_imports.py:24:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/ibis-project/ibis/blob/58174e5e19fb22ba48ad4722953fc88428292e11/ibis/expr/visualize.py#L105'>ibis/expr/visualize.py:105:5:</a> PLR1702 Too many nested blocks (6 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+40 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/dtypes/dtypes.py#L1218'>pandas/core/dtypes/dtypes.py:1218:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/generic.py#L7331'>pandas/core/generic.py:7331:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/generic.py#L7331'>pandas/core/generic.py:7331:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/generic.py#L7331'>pandas/core/generic.py:7331:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/indexes/multi.py#L3211'>pandas/core/indexes/multi.py:3211:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/indexing.py#L1828'>pandas/core/indexing.py:1828:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/indexing.py#L1828'>pandas/core/indexing.py:1828:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/internals/array_manager.py#L266'>pandas/core/internals/array_manager.py:266:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/internals/construction.py#L811'>pandas/core/internals/construction.py:811:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/reshape/merge.py#L1015'>pandas/core/reshape/merge.py:1015:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/reshape/merge.py#L1015'>pandas/core/reshape/merge.py:1015:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/strings/accessor.py#L281'>pandas/core/strings/accessor.py:281:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/tools/times.py#L82'>pandas/core/tools/times.py:82:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/common.py#L18'>pandas/core/window/common.py:18:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/common.py#L18'>pandas/core/window/common.py:18:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/numba_.py#L130'>pandas/core/window/numba_.py:130:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/numba_.py#L130'>pandas/core/window/numba_.py:130:9:</a> PLR1702 Too many nested blocks (8 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/numba_.py#L316'>pandas/core/window/numba_.py:316:9:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/numba_.py#L316'>pandas/core/window/numba_.py:316:9:</a> PLR1702 Too many nested blocks (8 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/online.py#L59'>pandas/core/window/online.py:59:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/pandas-dev/pandas/blob/622f31c9c455c64751b03b18e357b8f7bd1af0fd/pandas/core/window/online.py#L59'>pandas/core/window/online.py:59:9:</a> PLR1702 Too many nested blocks (6 > 5)
... 19 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L124'>rotkehlchen/chain/ethereum/modules/convex/decoder.py:124:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/chain/evm/node_inquirer.py#L937'>rotkehlchen/chain/evm/node_inquirer.py:937:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/chain/evm/node_inquirer.py#L937'>rotkehlchen/chain/evm/node_inquirer.py:937:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/chain/evm/node_inquirer.py#L937'>rotkehlchen/chain/evm/node_inquirer.py:937:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/externalapis/etherscan.py#L242'>rotkehlchen/externalapis/etherscan.py:242:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/tests/api/test_makerdao_dsr.py#L502'>rotkehlchen/tests/api/test_makerdao_dsr.py:502:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/tests/unit/test_tasks_manager.py#L183'>rotkehlchen/tests/unit/test_tasks_manager.py:183:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/tests/utils/blockchain.py#L223'>rotkehlchen/tests/utils/blockchain.py:223:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/tests/utils/blockchain.py#L223'>rotkehlchen/tests/utils/blockchain.py:223:9:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/rotki/rotki/blob/25f9d567b24bab38b8490bde9a9ac70f232dd332/rotkehlchen/tests/utils/blockchain.py#L223'>rotkehlchen/tests/utils/blockchain.py:223:9:</a> PLR1702 Too many nested blocks (7 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/scripts/setup/generate_secrets.py#L128'>scripts/setup/generate_secrets.py:128:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (6 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (7 > 5)
+ <a href='https://github.com/zulip/zulip/blob/ffc1056c457877987fb2edf81c510d65b82337cd/zerver/lib/events.py#L749'>zerver/lib/events.py:749:5:</a> PLR1702 Too many nested blocks (7 > 5)
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1702 | 95 | 95 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[pylint] - implement `PLR1702`/`too-many-nested-blocks`" to "[`pylint`] - implement `PLR1702`/`too-many-nested-blocks`" by @diceroll123 on 2024-01-20 22:56_

---

_Renamed from "[`pylint`] - implement `PLR1702`/`too-many-nested-blocks`" to "[`pylint`] - implement `too-many-nested-blocks` (`PLR1702`)" by @diceroll123 on 2024-01-21 04:33_

---

_@charliermarsh approved on 2024-01-24 19:24_

Thx!

---

_Label `rule` added by @charliermarsh on 2024-01-24 19:24_

---

_Label `preview` added by @charliermarsh on 2024-01-24 19:24_

---

_Renamed from "[`pylint`] - implement `too-many-nested-blocks` (`PLR1702`)" to "[`pylint`] Implement `too-many-nested-blocks` (`PLR1702`)" by @charliermarsh on 2024-01-24 19:25_

---

_Merged by @charliermarsh on 2024-01-24 19:30_

---

_Closed by @charliermarsh on 2024-01-24 19:30_

---
