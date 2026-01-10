```yaml
number: 10630
title: "[`refurb`] Implement `for-loop-writes` (`FURB122`)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: latyshev/furb122
created_at: 2024-03-27T17:02:06Z
updated_at: 2025-06-10T20:46:53Z
url: https://github.com/astral-sh/ruff/pull/10630
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] Implement `for-loop-writes` (`FURB122`)

---

_Pull request opened by @alex-700 on 2024-03-27 17:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Implement `for-loop-writes` (FURB122) lint
- https://github.com/astral-sh/ruff/issues/1348
- [original lint](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/writelines.py)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-27 17:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/dce84825ae9535af67f4035b3159dd5431d4641e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L147'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:147:21:</a> FURB122 [*] Use of `destination_file.write` in a for loop
+ <a href='https://github.com/apache/airflow/blob/dce84825ae9535af67f4035b3159dd5431d4641e/docs/exts/extra_files_with_substitutions.py#L41'>docs/exts/extra_files_with_substitutions.py:41:17:</a> FURB122 [*] Use of `output_file.write` in a for loop
+ <a href='https://github.com/apache/airflow/blob/dce84825ae9535af67f4035b3159dd5431d4641e/docs/exts/extra_files_with_substitutions.py#L51'>docs/exts/extra_files_with_substitutions.py:51:13:</a> FURB122 [*] Use of `output_file.write` in a for loop
+ <a href='https://github.com/apache/airflow/blob/dce84825ae9535af67f4035b3159dd5431d4641e/providers/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L233'>providers/src/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:233:13:</a> FURB122 [*] Use of `temp_file.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/249fdf444a7d5e1e4d06702d2dd6874c55b1b262/tests/integration_tests/core_tests.py#L443'>tests/integration_tests/core_tests.py:443:13:</a> FURB122 Use of `test_file.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L333'>scripts/milestone.py:333:9:</a> FURB122 [*] Use of `out.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/768a8a4381bda35a916f9de9f8e4b0cf64db8e9b/src/latch_cli/services/get_params.py#L178'>src/latch_cli/services/get_params.py:178:9:</a> FURB122 [*] Use of `f.write` in a for loop
+ <a href='https://github.com/latchbio/latch/blob/768a8a4381bda35a916f9de9f8e4b0cf64db8e9b/src/latch_cli/services/get_params.py#L180'>src/latch_cli/services/get_params.py:180:9:</a> FURB122 [*] Use of `f.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b870faec5acc717d704a7e2fe9eb2634a3522f09/tools/droplets/add_mentor.py#L69'>tools/droplets/add_mentor.py:69:13:</a> FURB122 [*] Use of `f.write` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/b870faec5acc717d704a7e2fe9eb2634a3522f09/zerver/lib/test_helpers.py#L572'>zerver/lib/test_helpers.py:572:13:</a> FURB122 [*] Use of `f.write` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/b870faec5acc717d704a7e2fe9eb2634a3522f09/zerver/lib/test_runner.py#L306'>zerver/lib/test_runner.py:306:17:</a> FURB122 [*] Use of `f.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/ea49b855610eb0101accd841375c94dd66e11258/astropy/samp/lockfile_helpers.py#L46'>astropy/samp/lockfile_helpers.py:46:5:</a> FURB122 [*] Use of `lockfile.write` in a for loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB122 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:13 on 2024-03-27 17:19_

```suggestion
/// When writing a batch of elements, it's more idiomatic to use a single method call,
/// `IOBase.writelines`, rather than write elements one by one.
```

---

_@VascoSch92 reviewed on 2024-03-27 17:19_

Really nice 😊👍 

---

_@alex-700 reviewed on 2024-03-27 18:24_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:13 on 2024-03-27 18:24_

Great catch, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-28 13:51_

---

_Label `rule` added by @charliermarsh on 2024-03-28 13:51_

---

_Label `preview` added by @charliermarsh on 2024-03-28 13:51_

---

_Renamed from "[`refurb`] Implement `for-loop-writes` (FURB122) lint" to "[`refurb`] Implement `for-loop-writes` (`FURB122`)" by @dylwil3 on 2025-01-16 20:56_

---

_Comment by @dylwil3 on 2025-01-16 21:02_

This is an excellent contribution! Thank you so much, and apologies for the long delay in resolving this PR.

---

_Merged by @dylwil3 on 2025-01-16 21:02_

---

_Closed by @dylwil3 on 2025-01-16 21:02_

---

_Branch deleted on 2025-06-10 20:46_

---
