```yaml
number: 6076
title: Avoid A003 violations for explicitly overridden methods
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/typing-extensions
created_at: 2023-07-25T16:05:55Z
updated_at: 2023-07-25T16:24:33Z
url: https://github.com/astral-sh/ruff/pull/6076
synced_at: 2026-01-12T03:30:22Z
```

# Avoid A003 violations for explicitly overridden methods

---

_Pull request opened by @charliermarsh on 2023-07-25 16:05_

## Summary

If a method is annotated with `@typing_extensions.override`, we should avoid flagging A003 on it. This isn't part of the standard library yet, but it's used to explicitly mark methods as overrides.


---

_@charliermarsh reviewed on 2023-07-25 16:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_builtins/rules/builtin_attribute_shadowing.rs`:98 on 2023-07-25 16:06_

(Same violation, but split into one method for attributes, another for methods.)

---

_Merged by @charliermarsh on 2023-07-25 16:21_

---

_Closed by @charliermarsh on 2023-07-25 16:21_

---

_Branch deleted on 2023-07-25 16:21_

---

_Comment by @github-actions[bot] on 2023-07-25 16:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
