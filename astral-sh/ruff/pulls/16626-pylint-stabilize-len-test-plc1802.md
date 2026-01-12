```yaml
number: 16626
title: "[`pylint`] Stabilize `len-test` (`PLC1802`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/plc1802-0.10
created_at: 2025-03-11T14:01:54Z
updated_at: 2025-03-11T15:30:45Z
url: https://github.com/astral-sh/ruff/pull/16626
synced_at: 2026-01-12T15:55:55Z
```

# [`pylint`] Stabilize `len-test` (`PLC1802`)

---

_@ntBre_

Summary
--

Stabilizes PLC1802. The tests were already in the right place, and I just tidied the docs a little bit.

Test Plan
--

1 issue closed 4 days after the rule was added, no other issues


---

_Label `rule` added by @ntBre on 2025-03-11 14:01_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 14:01_

---

_Comment by @github-actions[bot] on 2025-03-11 14:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+29 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1743'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1743:8:</a> PLC1802 [*] `len(success_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1750'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1750:8:</a> PLC1802 [*] `len(skipped_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1855'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1855:12:</a> PLC1802 [*] `len(success_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1862'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1862:12:</a> PLC1802 [*] `len(skipped_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/providers/amazon/src/airflow/providers/amazon/aws/hooks/glue.py#L268'>providers/amazon/src/airflow/providers/amazon/aws/hooks/glue.py:268:16:</a> PLC1802 [*] `len(fetched_logs)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/providers/sftp/src/airflow/providers/sftp/sensors/sftp.py#L132'>providers/sftp/src/airflow/providers/sftp/sensors/sftp.py:132:16:</a> PLC1802 [*] `len(files_found)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/9c4ec2097cdddd73f7462f4a8970bfa594605aae/providers/snowflake/src/airflow/providers/snowflake/operators/snowflake.py#L474'>providers/snowflake/src/airflow/providers/snowflake/operators/snowflake.py:474:20:</a> PLC1802 [*] `len(queries_in_progress)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/databases/filters.py#L98'>superset/databases/filters.py:98:16:</a> PLC1802 [*] `len(allowed_schemas)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 [*] `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L546'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:546:11:</a> PLC1802 [*] `len(ordered_raw_positions)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L559'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:559:16:</a> PLC1802 [*] `len(available_columns_index)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L68'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:68:24:</a> PLC1802 [*] `len(keys)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/check.py#L216'>src/bokeh/core/validation/check.py:216:27:</a> PLC1802 [*] `len(warnings)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/core/arrays/string_arrow.py#L284'>pandas/core/arrays/string_arrow.py:284:16:</a> PLC1802 [*] `len(value_set)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/core/groupby/generic.py#L2508'>pandas/core/groupby/generic.py:2508:16:</a> PLC1802 [*] `len(results)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/core/groupby/groupby.py#L5177'>pandas/core/groupby/groupby.py:5177:16:</a> PLC1802 [*] `len(to_coerce)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/core/internals/blocks.py#L811'>pandas/core/internals/blocks.py:811:16:</a> PLC1802 [*] `len(pairs)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/core/internals/construction.py#L867'>pandas/core/internals/construction.py:867:8:</a> PLC1802 [*] `len(contents)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/core/internals/managers.py#L1301'>pandas/core/internals/managers.py:1301:12:</a> PLC1802 [*] `len(removed_blknos)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/io/pytables.py#L1763'>pandas/io/pytables.py:1763:16:</a> PLC1802 [*] `len(lkeys)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/io/pytables.py#L4543'>pandas/io/pytables.py:4543:12:</a> PLC1802 [*] `len(masks)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/io/pytables.py#L4663'>pandas/io/pytables.py:4663:20:</a> PLC1802 [*] `len(groups)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/89bc204e94cda860085865081cd18cd1d130cec1/pandas/io/sql.py#L2618'>pandas/io/sql.py:2618:12:</a> PLC1802 [*] `len(ix_cols)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/d149ab353adcfa67693f692e3ee235888971e744/testing/_py/test_local.py#L218'>testing/_py/test_local.py:218:16:</a> PLC1802 [*] `len(lst)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/io/ascii/misc.py#L83'>astropy/io/ascii/misc.py:83:12:</a> PLC1802 [*] `len(first)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/units/core.py#L1331'>astropy/units/core.py:1331:16:</a> PLC1802 [*] `len(subresults)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/units/core.py#L2549'>astropy/units/core.py:2549:12:</a> PLC1802 [*] `len(names)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/wcs/wcs.py#L2950'>astropy/wcs/wcs.py:2950:16:</a> PLC1802 [*] `len(missing_keys)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/wcs/wcs.py#L3765'>astropy/wcs/wcs.py:3765:20:</a> PLC1802 [*] `len(content)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 29 | 29 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 14:20_

The ecosystem results look right to me. I checked that they were all `len(...)` calls and also that the `...` was a sequence. There was one [case](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/check.py#L216) where it wasn't obvious to me that it was a sequence, but it looks like the simple type inference looked at the class definition with an annotation and got it right.

---

_@MichaReiser approved on 2025-03-11 14:22_

---

_Merged by @ntBre on 2025-03-11 15:30_

---

_Closed by @ntBre on 2025-03-11 15:30_

---

_Branch deleted on 2025-03-11 15:30_

---
