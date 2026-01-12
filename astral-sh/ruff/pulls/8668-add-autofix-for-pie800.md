```yaml
number: 8668
title: Add autofix for PIE800
type: pull_request
state: merged
author: alanhdu
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alan/pie800
created_at: 2023-11-14T04:10:13Z
updated_at: 2023-11-16T02:33:18Z
url: https://github.com/astral-sh/ruff/pull/8668
synced_at: 2026-01-10T23:40:55Z
```

# Add autofix for PIE800

---

_Pull request opened by @alanhdu on 2023-11-14 04:10_

## Summary

This adds an autofix for PIE800 (unnecessary spread) -- whenever we see a `**{...}` inside another dictionary literal, just delete the `**{` and `}` to inline the key-value pairs. So `{"a": "b", **{"c": "d"}}` becomes just `{"a": "b", "c": "d"}`.

I have enabled this just for preview mode.

## Test Plan

Updated the preview snapshot test.

---

_Review comment by @alanhdu on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:58 on 2023-11-14 04:11_

I wasn't sure if there was a better way to do this -- the `key` is just `None`, and AFAICT nothing in the AST actually records the location of the `**`, so doing this `rfind` is the best way I could think of (at least w/o changing the AST).

---

_Review comment by @alanhdu on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__preview__PIE800_PIE800.py.snap`:82 on 2023-11-14 04:12_

Is there a requirement that the output be formatted nicely? The edit here is syntactically valid, but the formatting is going to be quite off after the fix if there are multiple indented keys.

---

_@alanhdu reviewed on 2023-11-14 04:12_

---

_Comment by @github-actions[bot] on 2023-11-14 04:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +6 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/tests/providers/common/sql/operators/test_sql_execute.py#L358'>tests/providers/common/sql/operators/test_sql_execute.py:358:49:</a> PIE800 Unnecessary spread `**`
+ <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/tests/providers/common/sql/operators/test_sql_execute.py#L358'>tests/providers/common/sql/operators/test_sql_execute.py:358:49:</a> PIE800 [*] Unnecessary spread `**`
- <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/tests/providers/docker/hooks/test_docker.py#L215'>tests/providers/docker/hooks/test_docker.py:215:29:</a> PIE800 Unnecessary spread `**`
+ <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/tests/providers/docker/hooks/test_docker.py#L215'>tests/providers/docker/hooks/test_docker.py:215:29:</a> PIE800 [*] Unnecessary spread `**`
- <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/tests/providers/docker/hooks/test_docker.py#L221'>tests/providers/docker/hooks/test_docker.py:221:29:</a> PIE800 Unnecessary spread `**`
+ <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/tests/providers/docker/hooks/test_docker.py#L221'>tests/providers/docker/hooks/test_docker.py:221:29:</a> PIE800 [*] Unnecessary spread `**`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE800 | 6 | 0 | 0 | 6 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2023-11-14 04:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:58 on 2023-11-14 04:27_

You could use our `SimpleTokenizer`, which is basically a stripped-down, zero-allocation lexer that's useful in cases like this. If you grep for `SimpleTokenizer::starts_at`, you can find some examples.

---

_@charliermarsh reviewed on 2023-11-14 04:32_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__preview__PIE800_PIE800.py.snap`:82 on 2023-11-14 04:32_

It's generally seen as "best effort" but not required.

---

_@charliermarsh reviewed on 2023-11-14 18:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:57 on 2023-11-14 18:19_

Is there any way to restructure this to use `SimpleTokenizer`, i.e., forwards lexing? We've considered removing the backwards lexer in the past since it's quite complex, so I'd prefer to minimize usages if possible.

---

_Review comment by @alanhdu on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:57 on 2023-11-14 18:57_

Yeah, I think that should be possible:

* For deleting `**{`, I can lex forward from the end of the previous value in the outer dictionary.
* For deleting the last `}`, I can lex forward from the last value of the inner dictionary.

I might have to change the function signature to passi n the whole dict (and not just the keys + values) to handle the case where `{**{"a": "b"}}` case where there's no previous value (in which case I'd lex forward from the dict start).

(might not get to this until tomorrow though).

---

_@alanhdu reviewed on 2023-11-14 18:57_

---

_@alanhdu reviewed on 2023-11-15 04:03_

---

_Review comment by @alanhdu on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:57 on 2023-11-15 04:03_

Ok -- I think this should work.

I ended up leaving the formatting fairly ugly -- the more I tried to make it look nice, the less confident I became that I handled all the edge-cases correctly (e.g. handling comments correctly, avoiding double commas, getting the indentation right, etc) so I ended up going for something simple and just letting an autoformatter figure it out afterwards.

I might try and take another crack at having the fix provide better formatted code later.

---

_@charliermarsh approved on 2023-11-15 18:05_

---

_@charliermarsh reviewed on 2023-11-15 18:05_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_spread.rs`:57 on 2023-11-15 18:05_

Thanks for doing a second pass here!

---

_Label `autofix` added by @charliermarsh on 2023-11-15 18:05_

---

_Label `preview` added by @charliermarsh on 2023-11-15 18:05_

---

_Merged by @charliermarsh on 2023-11-15 18:11_

---

_Closed by @charliermarsh on 2023-11-15 18:11_

---

_Branch deleted on 2023-11-16 02:33_

---
