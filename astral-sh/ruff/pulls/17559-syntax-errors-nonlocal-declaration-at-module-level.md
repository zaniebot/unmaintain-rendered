```yaml
number: 17559
title: "[syntax-errors] `nonlocal` declaration at module level"
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: apb
created_at: 2025-04-22T17:54:56Z
updated_at: 2025-04-25T12:38:49Z
url: https://github.com/astral-sh/ruff/pull/17559
synced_at: 2026-01-12T15:56:02Z
```

# [syntax-errors] `nonlocal` declaration at module level

---

_@abhijeetbodas2001_

## Summary

Part of #17412

Add a new compile-time syntax error for detecting `nonlocal` declarations at a module level.

## Test Plan

- Added new inline tests for the syntax error
- Updated existing tests for `nonlocal` statement parsing to be inside a function scope


---

_Review requested from @MichaReiser by @abhijeetbodas2001 on 2025-04-22 17:54_

---

_Review requested from @dhruvmanila by @abhijeetbodas2001 on 2025-04-22 17:54_

---

_Comment by @abhijeetbodas2001 on 2025-04-22 17:55_

@dhruvmanila could you review this?

---

_Assigned to @ntBre by @MichaReiser on 2025-04-23 05:58_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:902 on 2025-04-23 14:48_

I think we should leave these as inline tests. You can still nest them in functions like in the `test_case`s:

```rust
        // test_err nonlocal_stmt_trailing_comma
        // def _():
        //     nonlocal ,
        //     nonlocal x,
        //     nonlocal x, y,
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:902 on 2025-04-23 14:54_

Actually, I think if you implemented the test version of `in_module_scope` as `self.scopes.is_empty()` you could even add a `test_ok` inline case for the new error and exclusively use inline tests.

https://github.com/astral-sh/ruff/blob/e7f38fe74ba642cbd4cf286d8524f4172cbe30e2/crates/ruff_python_parser/tests/fixtures.rs#L540-L542

We currently only track function scopes in the test visitor, but I think that's okay. It wouldn't be hard to add support for classes, if we really wanted to test that too.

---

_@ntBre reviewed on 2025-04-23 14:55_

Awesome, thanks! I had one suggestion about where to put tests, but this looks great to me.

---

_Review request for @dhruvmanila removed by @dhruvmanila on 2025-04-23 15:14_

---

_Comment by @dhruvmanila on 2025-04-23 15:14_

(I'll leave this to Brent)

---

_@abhijeetbodas2001 reviewed on 2025-04-23 18:06_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/src/parser/statement.rs`:902 on 2025-04-23 18:06_

Ah, didn't know there's a separate visitor for running unit-tests.
I've reset the existing test to remain inline, and enclosed them in a function scope like you suggested.

---

_Comment by @abhijeetbodas2001 on 2025-04-23 18:10_

@ntBre Thanks for the review. I've addressed your comment and pushed again.

I have one question -- since it looks like this repo follows the squash+merge git workflow, should I be updating the PR description every time I push after addressing review comments (since this description will end up in the commit message)?

(Edit: updated the PR description)

---

_Review requested from @ntBre by @abhijeetbodas2001 on 2025-04-24 02:38_

---

_Comment by @abhijeetbodas2001 on 2025-04-24 03:55_

(Resolved conflicts in the last push)

---

_@ntBre approved on 2025-04-24 17:15_

This looks great, thanks!

---

_Comment by @ntBre on 2025-04-24 17:19_

> I have one question -- since it looks like this repo follows the squash+merge git workflow, should I be updating the PR description every time I push after addressing review comments (since this description will end up in the commit message)?

Great question! I would say not to worry _too_ much about it, but it is definitely nice to keep it updated if things change substantially. Your edits here look perfect.


---

_Comment by @github-actions[bot] on 2025-04-24 17:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -12 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:36:</a> AIR311 [*] `airflow.datasets.DatasetAny` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:36:</a> AIR311 `airflow.datasets.DatasetAny` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:48:</a> AIR311 [*] `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L353'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:353:48:</a> AIR311 `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L355'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:355:67:</a> AIR311 [*] `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L355'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:355:67:</a> AIR311 `airflow.datasets.DatasetAll` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L678'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:678:18:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L678'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:678:18:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L774'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:774:18:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/tests/unit/openlineage/plugins/test_utils.py#L774'>providers/openlineage/tests/unit/openlineage/plugins/test_utils.py:774:18:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L985'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:985:22:</a> AIR311 [*] `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
+ <a href='https://github.com/apache/airflow/blob/6a3bc01907b42aa67475ba0361d6b137f8fb51f6/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L985'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:985:22:</a> AIR311 `airflow.timetables.datasets.DatasetOrTimeSchedule` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 12 | 0 | 0 | 0 | 12 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[syntax-errors] nonlocal declaration at module level" to "[syntax-errors] `nonlocal` declaration at module level" by @ntBre on 2025-04-24 17:35_

---

_Label `rule` added by @ntBre on 2025-04-24 17:35_

---

_Label `preview` added by @ntBre on 2025-04-24 17:35_

---

_Merged by @ntBre on 2025-04-24 20:11_

---

_Closed by @ntBre on 2025-04-24 20:11_

---

_Branch deleted on 2025-04-25 03:48_

---

_Comment by @abhijeetbodas2001 on 2025-04-25 06:10_

> Great question! I would say not to worry too much about it, but it is definitely nice to keep it updated if things change substantially. Your edits here look perfect.

Got it, thanks!

Also wanted to mention that your detailed issue write up was really helpful from a new contributor (ie, me) POV, so thanks for that as well :smile: 

---

_Comment by @ntBre on 2025-04-25 12:38_

Thank you, that's great to hear! And thanks for the great PRs!

---
