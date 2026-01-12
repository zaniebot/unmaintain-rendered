```yaml
number: 7880
title: "add autofix for `PYI030`"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: PYI030-autofix
created_at: 2023-10-10T01:20:52Z
updated_at: 2023-10-11T06:45:51Z
url: https://github.com/astral-sh/ruff/pull/7880
synced_at: 2026-01-12T15:55:25Z
```

# add autofix for `PYI030`

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

Adds autofix to `PYI030`

Closes #7854. 

Unsure if the cloning method I chose is the best solution here, feel free to suggest alternatives!

## Test Plan

`cargo test` as well as manually

---

_Comment by @github-actions[bot] on 2023-10-10 01:38_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+4, -4)</summary>
<p>

<pre>
- [*] 15842 fixable with the `--fix` option (6478 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 15845 fixable with the `--fix` option (6478 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/apache/airflow/blob/9a297380558420cc2f73a409e350f333530e279e/airflow/cli/commands/task_command.py#L80'>airflow/cli/commands/task_command.py:80:21:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
+ <a href='https://github.com/apache/airflow/blob/9a297380558420cc2f73a409e350f333530e279e/airflow/cli/commands/task_command.py#L80'>airflow/cli/commands/task_command.py:80:21:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
- <a href='https://github.com/apache/airflow/blob/9a297380558420cc2f73a409e350f333530e279e/airflow/models/mappedoperator.py#L85'>airflow/models/mappedoperator.py:85:20:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
+ <a href='https://github.com/apache/airflow/blob/9a297380558420cc2f73a409e350f333530e279e/airflow/models/mappedoperator.py#L85'>airflow/models/mappedoperator.py:85:20:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
- <a href='https://github.com/apache/airflow/blob/9a297380558420cc2f73a409e350f333530e279e/airflow/providers_manager.py#L195'>airflow/providers_manager.py:195:24:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["source", "package"]`
+ <a href='https://github.com/apache/airflow/blob/9a297380558420cc2f73a409e350f333530e279e/airflow/providers_manager.py#L195'>airflow/providers_manager.py:195:24:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["source", "package"]`
</pre>

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

<pre>
- [*] 17725 fixable with the `--fix` option (4795 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 17726 fixable with the `--fix` option (4795 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/bokeh/bokeh/blob/cf57b9ae0073ecf0434a8c10002a70f6bf2079cd/src/bokeh/core/serialization.py#L149'>src/bokeh/core/serialization.py:149:25:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["bool", "object"]`
+ <a href='https://github.com/bokeh/bokeh/blob/cf57b9ae0073ecf0434a8c10002a70f6bf2079cd/src/bokeh/core/serialization.py#L149'>src/bokeh/core/serialization.py:149:25:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["bool", "object"]`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI030 | 8 | 4 | 4 |



---

_@charliermarsh reviewed on 2023-10-10 02:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:78 on 2023-10-10 02:31_

Do you mind gating this in an `if checker.patch(diagnostic.kind.rule()) { ... }` block? We allow users to turn autofixes on and off, and that's enforced through this pattern.

---

_@charliermarsh reviewed on 2023-10-10 02:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:79 on 2023-10-10 02:31_

Can this `.clone()` be removed? We don't use `literal_memebrs` again, so it should be fine to consume it here.

---

_@charliermarsh reviewed on 2023-10-10 02:32_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:44 on 2023-10-10 02:32_

Nit: "Replace with a single `Literal`" would better match our style for these messages.

---

_Label `autofix` added by @charliermarsh on 2023-10-10 03:05_

---

_@charliermarsh approved on 2023-10-10 03:05_

Thanks!

---

_Merged by @charliermarsh on 2023-10-10 03:15_

---

_Closed by @charliermarsh on 2023-10-10 03:15_

---

_Comment by @jamesbraza on 2023-10-10 04:17_

Thank you @diceroll123 and @charliermarsh!! 🥳 

---

_@T-256 reviewed on 2023-10-11 06:45_

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI030_PYI030.py.snap`:179 on 2023-10-11 06:45_

Here expected `Literal[1, 2] | str` is the correct fix (?)


---
