```yaml
number: 14488
title: "[`ruff`] Implement `invalid-assert-message-literal-argument` (`RUF040`)"
type: pull_request
state: merged
author: Lokejoke
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: non-string-literal-as-assert-message
created_at: 2024-11-20T13:46:44Z
updated_at: 2024-11-28T18:09:43Z
url: https://github.com/astral-sh/ruff/pull/14488
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Implement `invalid-assert-message-literal-argument` (`RUF040`)

---

_Pull request opened by @Lokejoke on 2024-11-20 13:46_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements new rule discussed [here](https://github.com/astral-sh/ruff/discussions/14449).
In short, it searches for assert messages which were unintentionally used as a expression to be matched against.  

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test` and review of `ruff-ecosystem`

<!-- How was it tested? -->


---

_Converted to draft by @Lokejoke on 2024-11-20 14:09_

---

_Renamed from "[`ruff`] Implement `non-string-literal-as-assert-message` (`RUF035`)" to "[`ruff`] Implement `non-string-literal-as-assert-message` (`RUF040`)" by @Lokejoke on 2024-11-20 14:31_

---

_Comment by @github-actions[bot] on 2024-11-20 14:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+11 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/providers/tests/amazon/aws/hooks/test_base_aws.py#L1077'>providers/tests/amazon/aws/hooks/test_base_aws.py:1077:24:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/providers/tests/edge/worker_api/routes/test_rpc_api.py#L219'>providers/tests/edge/worker_api/routes/test_rpc_api.py:219:35:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/providers/tests/google/cloud/sensors/test_gcs.py#L201'>providers/tests/google/cloud/sensors/test_gcs.py:201:30:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/providers/tests/google/cloud/sensors/test_gcs.py#L86'>providers/tests/google/cloud/sensors/test_gcs.py:86:30:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/providers/tests/google/common/hooks/test_base_google.py#L84'>providers/tests/google/common/hooks/test_base_google.py:84:24:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/tests/api_connexion/endpoints/test_task_instance_endpoint.py#L1370'>tests/api_connexion/endpoints/test_task_instance_endpoint.py:1370:38:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/tests/api_fastapi/core_api/routes/public/test_task_instances.py#L1973'>tests/api_fastapi/core_api/routes/public/test_task_instances.py:1973:38:</a> RUF040 Non-string literal used as assert message
+ <a href='https://github.com/apache/airflow/blob/1351b08d86fe70127cd24623a0dd8efc99fd25bd/tests/api_internal/endpoints/test_rpc_api_endpoint.py#L173'>tests/api_internal/endpoints/test_rpc_api_endpoint.py:173:31:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_state.py#L47'>tests/unit/bokeh/io/test_state.py:47:43:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/ee0902a832b7fa3e5821ada176566301791e09ec/pandas/tests/series/accessors/test_cat_accessor.py#L43'>pandas/tests/series/accessors/test_cat_accessor.py:43:37:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/2157caf87960d904c8547c9168c94a7d535f21e0/testing/_py/test_local.py#L218'>testing/_py/test_local.py:218:26:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF040 | 11 | 11 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @Lokejoke on 2024-11-20 14:45_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:14 on 2024-11-23 07:55_

I'm not sure we can speculate quite that much about the cause 😄 
How about just:

```rust
/// An assert message which is a non-string literal was likely intended
/// to be used in a comparison assertion, rather than as a message.
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:9 on 2024-11-23 07:58_

```diff
- /// Checks for uses of non-string literal as assert message.
+ /// Checks for the use of a non-string literal as an assert message.
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:30 on 2024-11-23 08:01_

It's a little confusing that `NonStringLiteral` can be parsed as either:

- non-string literal, or
- non - (string-literal)

the former means "a literal that is not a string literal" the latter would include like... arbitrary expressions.

Can we rename the rule to `literal-besides-string-as-assert-message` or `literal-other-than-string-as-assert-message`?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:50 on 2024-11-23 08:03_

Are we skipping bytes literals on purpose? I think that's okay, but maybe it should be documented.

---

_@dylwil3 requested changes on 2024-11-23 08:07_

This is a really clever rule idea, and the ecosystem checks all look like genuine bugs - great job! (May be worth filing some issues in those repos, actually 😄 )

I just have a quibble with the name of the rule (see the comment below), and a question about bytes literals, but otherwise this looks awesome. Thank you!

---

_Label `rule` added by @MichaReiser on 2024-11-25 09:41_

---

_Label `preview` added by @MichaReiser on 2024-11-25 09:41_

---

_@Lokejoke reviewed on 2024-11-25 12:17_

---

_Review comment by @Lokejoke on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:50 on 2024-11-25 12:17_

Bytes literals look like legitimate messages, but I’m unsure of their purpose.

---

_@Lokejoke reviewed on 2024-11-25 12:21_

---

_Review comment by @Lokejoke on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:30 on 2024-11-25 12:21_

literal-other-than-string-as-assert-message sounds ok

---

_@MichaReiser reviewed on 2024-11-25 12:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:30 on 2024-11-25 12:37_

sorry for joining the bikeshedding: How about `invalid-assert-message-literal-argument`

It's less specific because it doesn't mention string literals but I think that's a benefit if we decide to allow byte literals (although it's not clear to me why we should allow them because why? You'll have a bad time reading that message)

---

_Renamed from "[`ruff`] Implement `non-string-literal-as-assert-message` (`RUF040`)" to "[`ruff`] Implement `invalid-assert-message-literal-argument` (`RUF040`)" by @Lokejoke on 2024-11-25 16:58_

---

_Review requested from @dylwil3 by @MichaReiser on 2024-11-25 17:28_

---

_@Lokejoke reviewed on 2024-11-25 17:54_

---

_Review comment by @Lokejoke on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:50 on 2024-11-25 17:54_

I’ve changed my mind. After reviewing the `ecosystem` output, I’ve noticed that byte literals are also being misused.

---

_@dylwil3 reviewed on 2024-11-25 23:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/non_string_literal_as_assert_message.rs`:30 on 2024-11-25 23:38_

I accept this bike shed color begrudgingly since "invalid" seems a bit strong - but let's keep it for now, changing the human-readable names can be done at any time since it's not a breaking change.

---

_@dylwil3 approved on 2024-11-25 23:39_

LGTM, thanks for the great contribution!

---

_Merged by @dylwil3 on 2024-11-25 23:41_

---

_Closed by @dylwil3 on 2024-11-25 23:41_

---

_Comment by @Lokejoke on 2024-11-26 09:53_

Thank you for your guidance.

---

_Comment by @potiuk on 2024-11-28 18:09_

Very helpful - thanks for the heads up @Lokejoke and team :D  https://github.com/apache/airflow/pull/44460 is coming  in Airflow. 

BTW, Some of them were not really "bugs" (i.e. when we had both assert with == and literal added after , but it was good to fix those anyway (True as message printed when assert fails is rather useless hint).

---
