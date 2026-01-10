```yaml
number: 9880
title: "[`refurb`] Implement `readlines_in_for` lint (FURB129)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
assignees: []
merged: true
base: main
head: latyshev/furb129
created_at: 2024-02-07T21:16:19Z
updated_at: 2024-02-13T09:34:25Z
url: https://github.com/astral-sh/ruff/pull/9880
synced_at: 2026-01-10T22:57:09Z
```

# [`refurb`] Implement `readlines_in_for` lint (FURB129)

---

_Pull request opened by @alex-700 on 2024-02-07 21:16_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Implement [implicit readlines (FURB129)](https://github.com/dosisod/refurb/blob/master/refurb/checks/iterable/implicit_readlines.py) lint.

## Notes
I need a help/an opinion about suggested implementations.

This implementation differs from the original one from `refurb` in the following way. This implementation checks syntactically the call of the method with the name `readlines()` inside `for` {loop|generator expression}. The implementation from refurb also [checks](https://github.com/dosisod/refurb/blob/master/refurb/checks/iterable/implicit_readlines.py#L43) that callee is a variable with a type `io.TextIOWrapper` or `io.BufferedReader`. 

- I do not see a simple way to implement the same logic.
- The best I can have is something like
```rust
checker.semantic().binding(checker.semantic().resolve_name(attr_expr.value.as_name_expr()?)?).statement(checker.semantic())
```
and analyze cases. But this will be not about types, but about guessing the type by assignment (or with) expression.
- Also this logic has several false negatives, when the callee is not a variable, but the result of function call (e.g. `open(...)`).
- On the other side, maybe it is good to lint this on other things, where this suggestion is not safe, and push the developers to change their interfaces to be less surprising, comparing with the standard library.
- Anyway while the current implementation has false-positives (I mentioned some of them in the test) I marked the fixes to be unsafe. 


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-02-07 21:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/dev/breeze/src/airflow_breeze/global_constants.py#L361'>dev/breeze/src/airflow_breeze/global_constants.py:361:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/dev/perf/sql_queries.py#L143'>dev/perf/sql_queries.py:143:41:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/docs/exts/redirects.py#L43'>docs/exts/redirects.py:43:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/scripts/ci/pre_commit/pre_commit_newsfragments.py#L35'>scripts/ci/pre_commit/pre_commit_newsfragments.py:35:43:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/6754e9d83b89e9701c1b96bdc807a14e8c826d72/tests/system/providers/google/cloud/gcs/resources/transform_script.py#L26'>tests/system/providers/google/cloud/gcs/resources/transform_script.py:26:39:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/35098f49597895718343091881fbd6198bd2022d/tools/lib/provision_inner.py#L101'>tools/lib/provision_inner.py:101:51:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/b43fa662c2dbdf8e400563dbaa792476aeffc4a4/setup.py#L18'>setup.py:18:30:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB129 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-13 02:15_

---

_Label `rule` added by @charliermarsh on 2024-02-13 02:15_

---

_@charliermarsh approved on 2024-02-13 03:09_

Thanks! I added some extensions to our (basic) type inference system to support this.

---

_Merged by @charliermarsh on 2024-02-13 03:28_

---

_Closed by @charliermarsh on 2024-02-13 03:28_

---

_Branch deleted on 2024-02-13 09:34_

---
