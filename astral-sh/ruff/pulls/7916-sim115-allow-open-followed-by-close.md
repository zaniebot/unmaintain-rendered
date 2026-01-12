```yaml
number: 7916
title: "[SIM115] Allow `open` followed by `close`"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: SIM115-close
created_at: 2023-10-11T11:33:18Z
updated_at: 2023-10-11T14:02:20Z
url: https://github.com/astral-sh/ruff/pull/7916
synced_at: 2026-01-12T02:32:41Z
```

# [SIM115] Allow `open` followed by `close`

---

_Pull request opened by @harupy on 2023-10-11 11:33_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

https://github.com/astral-sh/ruff/discussions/7907

## Test Plan

<!-- How was it tested? -->

New test cases


---

_Comment by @github-actions[bot] on 2023-10-11 11:49_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -9, 0 error(s))

<details><summary>airflow (+0, -9)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/airflow/utils/log/file_processor_handler.py#L150'>airflow/utils/log/file_processor_handler.py:150:13:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/airflow/utils/log/file_task_handler.py#L482'>airflow/utils/log/file_task_handler.py:482:13:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/core/test_logging_config.py#L146'>tests/core/test_logging_config.py:146:17:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/core/test_logging_config.py#L148'>tests/core/test_logging_config.py:148:13:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/providers/apache/beam/operators/test_beam.py#L433'>tests/providers/apache/beam/operators/test_beam.py:433:13:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/providers/apache/beam/operators/test_beam.py#L598'>tests/providers/apache/beam/operators/test_beam.py:598:13:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/sensors/test_filesystem.py#L103'>tests/sensors/test_filesystem.py:103:13:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/sensors/test_filesystem.py#L162'>tests/sensors/test_filesystem.py:162:17:</a> SIM115 Use context handler for opening files
- <a href='https://github.com/apache/airflow/blob/73dd877961cfaca0d29f127b0d868308d174bcd1/tests/sensors/test_filesystem.py#L181'>tests/sensors/test_filesystem.py:181:13:</a> SIM115 Use context handler for opening files
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM115 | 9 | 0 | 9 |



---

_Review comment by @konstin on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:47 on 2023-10-11 12:11_

Could you add a comment why this combination is needed?

---

_@konstin approved on 2023-10-11 12:11_

---

_Review comment by @harupy on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:47 on 2023-10-11 12:26_

```suggestion
# OK (file is closed immdeidately)
open("filename").close()
```

comment like this?

---

_@harupy reviewed on 2023-10-11 12:26_

---

_@konstin reviewed on 2023-10-11 12:34_

---

_Review comment by @konstin on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:47 on 2023-10-11 12:34_

More like "Used to clear an existing file (https://stackoverflow.com/a/2769090/3549270)"

There's `Path.touch` now but i'm surprised there's nothing similar from clearing a file.

---

_Comment by @konstin on 2023-10-11 12:36_

For append instead of write cases, e.g. https://github.com/apache/airflow/blob/e239dcf644b6e68896923d84dd49e9cbb6271373/airflow/utils/log/file_processor_handler.py#L150, i'd recommend using `Path.touch` instead, but this might better be a separate rule.

---

_Review comment by @harupy on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:46 on 2023-10-11 13:43_

```suggestion
# OK (quick one-liner to clear file contents)
```

---

_@harupy reviewed on 2023-10-11 13:43_

---

_Label `bug` added by @charliermarsh on 2023-10-11 13:46_

---

_Merged by @charliermarsh on 2023-10-11 13:53_

---

_Closed by @charliermarsh on 2023-10-11 13:53_

---

_Branch deleted on 2023-10-11 13:55_

---
