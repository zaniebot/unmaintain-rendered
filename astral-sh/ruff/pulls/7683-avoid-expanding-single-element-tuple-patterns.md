```yaml
number: 7683
title: Avoid expanding single-element tuple patterns
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/pat
created_at: 2023-09-27T22:38:53Z
updated_at: 2023-09-28T07:05:04Z
url: https://github.com/astral-sh/ruff/pull/7683
synced_at: 2026-01-12T15:55:24Z
```

# Avoid expanding single-element tuple patterns

---

_@charliermarsh_

## Summary

The formatting for tuple patterns is now intended to match that of `for` loops:

- Always parenthesize single-element tuples.
- Don't break on the trailing comma in single-element tuples.
- For other tuples, preserve the parentheses, and insert if-breaks.

Closes https://github.com/astral-sh/ruff/issues/7681.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-09-27 22:38_

---

_Merged by @charliermarsh on 2023-09-27 23:57_

---

_Closed by @charliermarsh on 2023-09-27 23:57_

---

_Branch deleted on 2023-09-27 23:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:507 on 2023-09-28 07:01_

Can we add some more tests with comments?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_sequence.rs`:25 on 2023-09-28 07:01_

This comment seems to miss at least a verb.

---

_@MichaReiser reviewed on 2023-09-28 07:05_

---
