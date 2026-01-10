```yaml
number: 10830
title: Add tests for class definition
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/class-def
created_at: 2024-04-08T05:52:05Z
updated_at: 2024-04-09T15:56:41Z
url: https://github.com/astral-sh/ruff/pull/10830
synced_at: 2026-01-10T22:37:01Z
```

# Add tests for class definition

---

_Pull request opened by @dhruvmanila on 2024-04-08 05:52_

## Summary

This PR adds test cases for class definition.

---

_Label `parser` added by @dhruvmanila on 2024-04-08 05:52_

---

_Label `testing` added by @dhruvmanila on 2024-04-08 05:52_

---

_Marked ready for review by @dhruvmanila on 2024-04-08 12:26_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-08 12:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1625 on 2024-04-09 08:37_

Nité Consider adding a test with a base class (except if that's covered in another test)

---

_@MichaReiser reviewed on 2024-04-09 08:37_

---

_@MichaReiser approved on 2024-04-09 08:37_

---

_@dhruvmanila reviewed on 2024-04-09 15:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:1625 on 2024-04-09 15:56_

It's covered in the `resources/valid/statement/class.py` file.

---

_Merged by @dhruvmanila on 2024-04-09 15:56_

---

_Closed by @dhruvmanila on 2024-04-09 15:56_

---

_Branch deleted on 2024-04-09 15:56_

---
