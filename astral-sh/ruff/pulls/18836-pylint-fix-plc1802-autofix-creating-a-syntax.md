```yaml
number: 18836
title: "[`pylint`] Fix `PLC1802` autofix creating a syntax error and mark autofix as unsafe if there's comments in the `len` call"
type: pull_request
state: merged
author: LaBatata101
labels: []
assignees: []
merged: true
base: main
head: fix-PLC1802
created_at: 2025-06-20T21:17:12Z
updated_at: 2025-06-23T00:40:49Z
url: https://github.com/astral-sh/ruff/pull/18836
synced_at: 2026-01-10T18:39:09Z
```

# [`pylint`] Fix `PLC1802` autofix creating a syntax error and mark autofix as unsafe if there's comments in the `len` call

---

_Pull request opened by @LaBatata101 on 2025-06-20 21:17_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
I've also found another bug while fixing this, where the diagnostic would not trigger if the `len` call argument variable was shadowed. This fixed a few false negatives in the test cases.
Example:
```python
fruits = []
fruits = []
if len(fruits):  # comment
    ...
```

Fixes #18811
Fixes #18812
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-20 21:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -2 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 [*] `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L91'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:91:20:</a> PLC1802 [*] `len(keys)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/frame.py#L7238'>pandas/core/frame.py:7238:14:</a> PLC1802 [*] `len(by)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/indexes/base.py#L7621'>pandas/core/indexes/base.py:7621:12:</a> PLC1802 [*] `len(index_like)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/base_parser.py#L246'>pandas/io/parsers/base_parser.py:246:12:</a> PLC1802 [*] `len(ic)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/python_parser.py#L284'>pandas/io/parsers/python_parser.py:284:16:</a> PLC1802 [*] `len(content)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/tests/runner.py#L547'>astropy/tests/runner.py:547:16:</a> PLC1802 [*] `len(paths)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/units/core.py#L1328'>astropy/units/core.py:1328:12:</a> PLC1802 [*] `len(results)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 9 | 7 | 0 | 0 | 2 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -2 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 [*] `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L91'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:91:20:</a> PLC1802 [*] `len(keys)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/frame.py#L7238'>pandas/core/frame.py:7238:14:</a> PLC1802 [*] `len(by)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/indexes/base.py#L7621'>pandas/core/indexes/base.py:7621:12:</a> PLC1802 [*] `len(index_like)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/base_parser.py#L246'>pandas/io/parsers/base_parser.py:246:12:</a> PLC1802 [*] `len(ic)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/python_parser.py#L284'>pandas/io/parsers/python_parser.py:284:16:</a> PLC1802 [*] `len(content)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/tests/runner.py#L547'>astropy/tests/runner.py:547:16:</a> PLC1802 [*] `len(paths)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/units/core.py#L1328'>astropy/units/core.py:1328:12:</a> PLC1802 [*] `len(results)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 9 | 7 | 0 | 0 | 2 |

</p>
</details>




---

_@charliermarsh approved on 2025-06-23 00:27_

Thanks!

---

_@charliermarsh reviewed on 2025-06-23 00:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/len_test.rs`:47 on 2025-06-23 00:28_

```suggestion
/// This rule's fix is marked as unsafe when the `len` call includes a comment,
/// as the comment would be removed.
```

---

_@charliermarsh reviewed on 2025-06-23 00:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/len_test.rs`:49 on 2025-06-23 00:29_

```suggestion
```

---

_Merged by @charliermarsh on 2025-06-23 00:32_

---

_Closed by @charliermarsh on 2025-06-23 00:32_

---

_Branch deleted on 2025-06-23 00:34_

---
