```yaml
number: 10382
title: "[`pycodestyle`] Do not ignore lines before the first logical line in blank lines rules."
type: pull_request
state: merged
author: hoel-bagard
labels:
  - bug
assignees: []
merged: true
base: main
head: hoel/blank-lines-handle-first-line
created_at: 2024-03-13T13:18:47Z
updated_at: 2024-03-14T12:26:25Z
url: https://github.com/astral-sh/ruff/pull/10382
synced_at: 2026-01-12T15:55:32Z
```

# [`pycodestyle`] Do not ignore lines before the first logical line in blank lines rules.

---

_@hoel-bagard_

## Summary

Ignoring all lines until the first logical line does not match the behavior from pycodestyle. This PR therefore removes the `if state.is_not_first_logical_line` skipping the line check before the first logical line, and applies it only to `E302`.

For example, in the snippet below a rule violation should be detected on the second comment and on the import.

```python
# first comment




# second comment




import foo
```

Fixes #10374

## Test Plan

It was tested on the example given on the issue, and by running cargo test. Since this is an issue about the first line(s) of a file I did not add a fixture for it.
The ruff-ecosystem check should help verify that the PR does not have unintended side-effects.

---

_Comment by @github-actions[bot] on 2024-03-13 13:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @hoel-bagard on 2024-03-13 13:52_

---

_@dhruvmanila reviewed on 2024-03-13 13:58_

Wow, thanks for the quick fix. Can you add some test cases for this? Ideally, separate test cases which tests against comments, docstrings, a random statement and expression.

---

_Label `bug` added by @dhruvmanila on 2024-03-13 13:58_

---

_Comment by @dhruvmanila on 2024-03-13 13:59_

I agree with your test plan although I think it would be useful to avoid any potential regression in the future. It's always good to have a test case for a bug fix PR which fails on `main` and passes on this branch :)

---

_Comment by @hoel-bagard on 2024-03-14 04:50_

@dhruvmanila I've added a few fixtures as you requested. Since `E302` and `E303` are the only rules that should be able to trigger on the second line, I only added snapshots for those rules.

Should I also add snapshots for the other rules to check that no false positive is introduced in the future ?

---

_@dhruvmanila approved on 2024-03-14 08:34_

Thank you! The test cases are great

---

_Merged by @dhruvmanila on 2024-03-14 08:35_

---

_Closed by @dhruvmanila on 2024-03-14 08:35_

---

_Branch deleted on 2024-03-14 12:26_

---
