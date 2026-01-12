```yaml
number: 12113
title: "[`flake8-bugbear`] Implement mutable-contextvar-default (B039)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/B039
created_at: 2024-06-30T21:39:54Z
updated_at: 2024-07-05T13:40:41Z
url: https://github.com/astral-sh/ruff/pull/12113
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-bugbear`] Implement mutable-contextvar-default (B039)

---

_@tjkuson_

## Summary

Implement mutable-contextvar-default (B039) which was added to flake8-bugbear in https://github.com/PyCQA/flake8-bugbear/pull/476.

This rule is similar to [mutable-argument-default (B006)](https://docs.astral.sh/ruff/rules/mutable-argument-default) and [function-call-in-default-argument (B008)](https://docs.astral.sh/ruff/rules/function-call-in-default-argument), except that it checks the `default` keyword argument to `contextvars.ContextVar`.

```
B039.py:19:26: B039 Do not use mutable data structures for ContextVar defaults
   |
18 | # Bad
19 | ContextVar("cv", default=[])
   |                          ^^ B039
20 | ContextVar("cv", default={})
21 | ContextVar("cv", default=list())
   |
   = help: Replace with `None`; initialize with `.set()` after checking for `None`
```

In the upstream flake8-plugin, this rule is written expressly as a corollary to B008 and shares much of its logic. Likewise, this implementation reuses the logic of the Ruff implementation of B008, namely

https://github.com/astral-sh/ruff/blob/f765d194028ef0ebd01ef0a21e30732d4d5ff184/crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs#L104-L106

and 

https://github.com/astral-sh/ruff/blob/f765d194028ef0ebd01ef0a21e30732d4d5ff184/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs#L106

Thus, this rule deliberately replicates B006's and B008's heuristics. For example, this rule assumes that all functions are mutable unless otherwise qualified. If improvements are to be made to B039 heuristics, they should probably be made to B006 and B008 as well (whilst trying to match the upstream implementation).

This rule does not have an autofix as it is unknown where the ContextVar next used (and it might not be within the same file).

Closes #12054

## Test Plan

`cargo nextest run`


---

_Marked ready for review by @tjkuson on 2024-06-30 21:46_

---

_@charliermarsh approved on 2024-07-01 01:51_

Thank you!

---

_Label `rule` added by @charliermarsh on 2024-07-01 01:52_

---

_Label `preview` added by @charliermarsh on 2024-07-01 01:52_

---

_Merged by @charliermarsh on 2024-07-01 01:55_

---

_Closed by @charliermarsh on 2024-07-01 01:55_

---

_Branch deleted on 2024-07-05 13:40_

---
