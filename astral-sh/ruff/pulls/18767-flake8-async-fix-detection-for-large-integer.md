```yaml
number: 18767
title: "[flake8-async] fix detection for large integer sleep durations in `ASYNC116` rule"
type: pull_request
state: merged
author: chirizxc
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/async116
created_at: 2025-06-18T20:03:40Z
updated_at: 2025-06-19T09:43:32Z
url: https://github.com/astral-sh/ruff/pull/18767
synced_at: 2026-01-12T15:56:25Z
```

# [flake8-async] fix detection for large integer sleep durations in `ASYNC116` rule

---

_@chirizxc_


<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #18762
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-18 20:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e614be661b70c7102495f39aa44d9e5f3a0f7fcb/devel-common/src/tests_common/test_utils/gcp_system_helpers.py#L175'>devel-common/src/tests_common/test_utils/gcp_system_helpers.py:175:32:</a> RUF064 Non-octal mode
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF064 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "[flake8-async] аix detection for large integer sleep durations in `ASYNC116` rule" to "[flake8-async] fix detection for large integer sleep durations in `ASYNC116` rule" by @chirizxc on 2025-06-18 20:30_

---

_Label `rule` added by @MichaReiser on 2025-06-19 09:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs`:105 on 2025-06-19 09:34_

```suggestion
            _ => {} // The number is too large, and more than 24 hours
```

---

_@MichaReiser reviewed on 2025-06-19 09:34_

---

_@MichaReiser approved on 2025-06-19 09:35_

Thank you

---

_Merged by @MichaReiser on 2025-06-19 09:37_

---

_Closed by @MichaReiser on 2025-06-19 09:37_

---

_Branch deleted on 2025-06-19 09:37_

---
