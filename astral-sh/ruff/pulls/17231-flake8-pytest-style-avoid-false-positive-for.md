```yaml
number: 17231
title: "[`flake8-pytest-style`] Avoid false positive for legacy form of `pytest.raises` (`PT011`)"
type: pull_request
state: merged
author: twentyone212121
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-issue-17026
created_at: 2025-04-06T14:38:25Z
updated_at: 2025-04-08T07:24:47Z
url: https://github.com/astral-sh/ruff/pull/17231
synced_at: 2026-01-12T15:56:01Z
```

# [`flake8-pytest-style`] Avoid false positive for legacy form of `pytest.raises` (`PT011`)

---

_@twentyone212121_

This fix closes #17026 

## Summary

The check for the `PytestRaisesTooBroad` rule is now skipped if there is a second positional argument present, which means `pytest.raises` is used as a function.

## Test Plan

Tested on the example from the issue, which now passes the check.
```Python3
pytest.raises(Exception, func, *func_args, **func_kwargs).match("error message")
```


---

_Label `rule` added by @MichaReiser on 2025-04-07 06:41_

---

_Comment by @MichaReiser on 2025-04-07 06:43_

Thanks. Could you add a test example to https://github.com/astral-sh/ruff/blob/2b6d66b793adc9e0cc50817b59187b41f39f7691/crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT011.py#L1-L0 with your example from the test plan

---

_Comment by @twentyone212121 on 2025-04-07 09:18_

I added an ok test example. The diff in snapshots is big because error tests are moved by 7 lines

---

_Comment by @github-actions[bot] on 2025-04-07 21:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @Copilot by @MichaReiser on 2025-04-08 06:58_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-04-08 06:58_



Copilot reviewed 2 out of 6 changed files in this pull request and generated no comments.

<details>
<summary>Files not reviewed (4)</summary>

* **crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT011_default.snap**: Language not supported
* **crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT011_extend_broad_exceptions.snap**: Language not supported
* **crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT011_glob_all.snap**: Language not supported
* **crates/ruff_linter/src/rules/flake8_pytest_style/snapshots/ruff_linter__rules__flake8_pytest_style__tests__PT011_glob_prefix.snap**: Language not supported
</details>

<details>
<summary>Comments suppressed due to low confidence (1)</summary>

**crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs:192**
* [nitpick] Using the string literal "func" with a hard-coded index (1) may reduce readability. Consider defining a constant for this argument name and index to clarify its purpose when detecting the second overload.
```
if call.arguments.find_argument("func", 1).is_none() {
```
</details>



---

_Merged by @MichaReiser on 2025-04-08 07:24_

---

_Closed by @MichaReiser on 2025-04-08 07:24_

---
