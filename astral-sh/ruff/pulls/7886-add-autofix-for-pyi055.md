```yaml
number: 7886
title: "add autofix for `PYI055`"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: PYI055
created_at: 2023-10-10T05:21:51Z
updated_at: 2023-10-13T01:02:54Z
url: https://github.com/astral-sh/ruff/pull/7886
synced_at: 2026-01-12T15:55:25Z
```

# add autofix for `PYI055`

---

_@diceroll123_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds autofix for `PYI055`

Questions:
- is it intended that only the code within the `def func():` gets fixed? 🤔 I feel like some of the top-level variables would also be affected.
- should `w: builtins.type[int] | builtins.type[str] | builtins.type[complex]` turn into `w: builtins.type[int | str | complex]`?
  - or `w: type[int | str | complex]`?

## Test Plan

<!-- How was it tested? -->

`cargo test`, and manually.

---

_Comment by @github-actions[bot] on 2023-10-10 05:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+4, -4)</summary>
<p>

<pre>
- [*] 16066 fixable with the `--fix` option (6343 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 16069 fixable with the `--fix` option (6343 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1580'>airflow/providers/google/cloud/hooks/bigquery.py:1580:35:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
+ <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1580'>airflow/providers/google/cloud/hooks/bigquery.py:1580:35:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
- <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1587'>airflow/providers/google/cloud/hooks/bigquery.py:1587:14:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
+ <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1587'>airflow/providers/google/cloud/hooks/bigquery.py:1587:14:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
- <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/smtp/hooks/smtp.py#L103'>airflow/providers/smtp/hooks/smtp.py:103:15:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[smtplib.SMTP_SSL | smtplib.SMTP]`.
+ <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/smtp/hooks/smtp.py#L103'>airflow/providers/smtp/hooks/smtp.py:103:15:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[smtplib.SMTP_SSL | smtplib.SMTP]`.
</pre>

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

<pre>
- [*] 17800 fixable with the `--fix` option (4727 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 17801 fixable with the `--fix` option (4727 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/src/bokeh/core/validation/decorators.py#L67'>src/bokeh/core/validation/decorators.py:67:13:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[Error | Warning]`.
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/src/bokeh/core/validation/decorators.py#L67'>src/bokeh/core/validation/decorators.py:67:13:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[Error | Warning]`.
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI055 | 8 | 4 | 4 |



---

_Marked ready for review by @diceroll123 on 2023-10-11 03:50_

---

_@diceroll123 reviewed on 2023-10-11 03:51_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:163 on 2023-10-11 03:51_

is there a shorthand for any of this?

---

_@charliermarsh reviewed on 2023-10-11 03:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:163 on 2023-10-11 03:58_

No, it's bad 😞 

---

_@diceroll123 reviewed on 2023-10-11 03:59_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:163 on 2023-10-11 03:59_

That is _NOT_ what I wanted to hear! 🤣 Thanks anyways.

---

_@charliermarsh reviewed on 2023-10-11 04:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:163 on 2023-10-11 04:04_

Hopefully clear that I mean the abstractions are bad, and not your code :)

---

_@diceroll123 reviewed on 2023-10-11 04:04_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:163 on 2023-10-11 04:04_

🤣 of course.

...but why not both?

---

_Comment by @charliermarsh on 2023-10-11 04:17_

I think `w: builtins.type[int] | builtins.type[str] | builtins.type[complex]` should turn into `w: builtins.type[int | str | complex]`.

Is your first question still relevant? It looks like stuff outside of the `def` is being flagged in the snapshot.

---

_Comment by @diceroll123 on 2023-10-11 04:20_

> I think `w: builtins.type[int] | builtins.type[str] | builtins.type[complex]` should turn into `w: builtins.type[int | str | complex]`.
> 
> Is your first question still relevant? It looks like stuff outside of the `def` is being flagged in the snapshot.

mmm, that answers my question! I'll dig into that. Thanks!

---

_Comment by @charliermarsh on 2023-10-11 04:23_

On second thought, it's probably not worth the complexity. But we should ensure that `checker.is_builtin("type")` before adding the autofix.

---

_Comment by @diceroll123 on 2023-10-11 04:32_

> On second thought, it's probably not worth the complexity. But we should ensure that `checker.is_builtin("type")` before adding the autofix.

```rust
checker
        .semantic()
        .resolve_call_path(subscript.value.as_ref())
        .is_some_and(|call_path| matches!(call_path.as_slice(), ["" | "builtins", "type"]))
```

is this code starting on line 83 not doing that? or is it not explicit enough?

---

_Label `autofix` added by @charliermarsh on 2023-10-13 00:46_

---

_Comment by @charliermarsh on 2023-10-13 00:46_

It's not quite sufficient, because that would also match `builtins.type`. I just added a guard to check the builtin explicitly prior to performing the fix.

---

_Merged by @charliermarsh on 2023-10-13 00:56_

---

_Closed by @charliermarsh on 2023-10-13 00:56_

---
