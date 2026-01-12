```yaml
number: 7521
title: "Add `Expr::Name` checks to rules which use `is_logger_candidate` "
type: pull_request
state: merged
author: qdegraaf
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/expandisloggingcandidate
created_at: 2023-09-19T15:23:59Z
updated_at: 2023-09-27T09:00:12Z
url: https://github.com/astral-sh/ruff/pull/7521
synced_at: 2026-01-12T02:39:10Z
```

# Add `Expr::Name` checks to rules which use `is_logger_candidate` 

---

_Pull request opened by @qdegraaf on 2023-09-19 15:23_

## Summary

Expands several rules to also check for `Expr::Name` values. As they would previously not consider:
```python
from logging import error

error("foo")
```
as potential violations
```python
import logging

logging.error("foo")
```
as potential violations leading to inconsistent behaviour. 

The rules impacted are:

- `BLE001`
- `TRY400`
- `TRY401`
- `PLE1205`
- `PLE1206`
- `LOG007`
- `G001`-`G004`
- `G101`
- `G201`
- `G202`

## Test Plan

Fixtures for all impacted rules expanded. 

## Issue Link

Refers: https://github.com/astral-sh/ruff/issues/7502


---

_Comment by @github-actions[bot] on 2023-09-19 15:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-09-19 17:16_

So I think there's a little more to do here, but it's not clear from this PR because there's actually a bug in `LOG007`. Lemme push a fix, then I'll come back and explain.

---

_Comment by @charliermarsh on 2023-09-19 17:20_

Take a look at https://github.com/astral-sh/ruff/pull/7524. The issue is that we assume all calls are done on attributes, so that we can resolve the attribute to, e.g., `.exception` or `.info` or whatever. The same pattern is used in `logging_call`, in `crates/ruff/src/rules/flake8_logging_format/rules/logging_call.rs`.

We may want to change `logging_call` (and the `LOG007` method) to match on either name or attribute, and use separate paths to determine whether it's a logger and, if so, what the log level is. _Or_ we could try changing _this_ method to return the logging attribute too.


---

_Review requested from @charliermarsh by @charliermarsh on 2023-09-19 17:20_

---

_Label `bug` added by @charliermarsh on 2023-09-19 17:20_

---

_Comment by @qdegraaf on 2023-09-19 22:27_

Interesting, thanks for the taking the time to explain and clarifying my previous misinterpretation of `is_logger_candidate` in `LOG007`. I'll have a look tomorrow if I can implement what you're suggesting. Will mark as draft until then.

---

_Converted to draft by @qdegraaf on 2023-09-19 22:27_

---

_Renamed from "Expand `is_logger_candidate` to check `Expr::Name` expressions" to "Expand `LOG007` and `logging_call.rs` to check `Expr::Name` expressions" by @qdegraaf on 2023-09-20 14:18_

---

_Marked ready for review by @qdegraaf on 2023-09-20 14:19_

---

_@charliermarsh reviewed on 2023-09-20 20:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:172 on 2023-09-20 20:29_

@qdegraaf - I changed this to always use `resolve_call_path`. I think we want this to _only_ match the logging imports.

---

_Comment by @charliermarsh on 2023-09-20 20:35_

@qdegraaf - Nice! I think we need to extend this to a few more call sites. Are you able to look into the other usages of `is_logger_candidate` and apply similar logic? E.g., in `blind_except`, in the callers of `LoggerCandidateVisitor`, and so on. If not, we can merge and fix those up later, but I believe they do also need refinement.

---

_Comment by @qdegraaf on 2023-09-20 22:17_

Happy to add those to the PR! Will have a look tomorrow

---

_Renamed from "Expand `LOG007` and `logging_call.rs` to check `Expr::Name` expressions" to "Add `Expr::Name` checks to rules which use `is_logger_candidate` " by @qdegraaf on 2023-09-21 11:36_

---

_Comment by @charliermarsh on 2023-09-27 00:01_

Thank you!

---

_@charliermarsh approved on 2023-09-27 00:01_

---

_Merged by @charliermarsh on 2023-09-27 00:21_

---

_Closed by @charliermarsh on 2023-09-27 00:21_

---

_Branch deleted on 2023-09-27 09:00_

---
