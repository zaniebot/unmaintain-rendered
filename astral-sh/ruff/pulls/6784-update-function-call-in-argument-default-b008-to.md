```yaml
number: 6784
title: "Update `function-call-in-argument-default` (`B008`) to ignore arguments with immutable annotations"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
assignees: []
merged: true
base: main
head: rule/b008-ignore-immutable
created_at: 2023-08-22T17:31:49Z
updated_at: 2023-08-24T16:13:00Z
url: https://github.com/astral-sh/ruff/pull/6784
synced_at: 2026-01-12T02:45:38Z
```

# Update `function-call-in-argument-default` (`B008`) to ignore arguments with immutable annotations

---

_Pull request opened by @zanieb on 2023-08-22 17:31_

Extends #6781 
Part of https://github.com/astral-sh/ruff/issues/3762
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Allows calls in argument defaults if the argument is annotated as an immutable type to avoid false positives.

## Test Plan

<!-- How was it tested? -->

Snapshots


---

_Comment by @zanieb on 2023-08-22 17:38_

Stuck dealing with the borrow checker

```
error[E0502]: cannot borrow `checker.diagnostics` as mutable because it is also borrowed as immutable
   --> crates/ruff/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs:148:9
    |
122 |             ArgumentDefaultVisitor::new(checker.semantic(), extend_immutable_calls.as_slice());
    |                                         ------------------ immutable borrow occurs here
...
148 |         checker.diagnostics.push(Diagnostic::new(check, range));
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ mutable borrow occurs here
149 |     }
150 | }
    | - immutable borrow might be used here, when `extend_immutable_calls` is dropped and runs the `Drop` code for type `std::vec::Vec`


---

_Marked ready for review by @zanieb on 2023-08-23 15:51_

---

_Label `rule` added by @zanieb on 2023-08-23 16:24_

---

_@konstin approved on 2023-08-24 11:56_

---

_Merged by @zanieb on 2023-08-24 16:12_

---

_Closed by @zanieb on 2023-08-24 16:12_

---

_Branch deleted on 2023-08-24 16:13_

---
