```yaml
number: 8933
title: "Use correct range for `TRIO115` fix"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/fix-trio115
created_at: 2023-12-01T01:37:33Z
updated_at: 2023-12-01T03:03:41Z
url: https://github.com/astral-sh/ruff/pull/8933
synced_at: 2026-01-12T15:55:27Z
```

# Use correct range for `TRIO115` fix

---

_@dhruvmanila_

## Summary

This PR fixes the bug where the autofix for `TRIO115` was taking the entire arguments range for the fix which included the parenthesis as well. This means that the fix would remove the arguments and the parenthesis. The fix is to use the correct range.

fixes: #8713 

## Test Plan

Update existing snapshots :)


---

_Comment by @dhruvmanila on 2023-12-01 01:37_

Current dependencies on/for this PR:
* main
  * **PR #8933** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8933?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8933?utm_source=stack-comment).

---

_@zanieb approved on 2023-12-01 01:38_

---

_Label `bug` added by @dhruvmanila on 2023-12-01 01:39_

---

_Merged by @dhruvmanila on 2023-12-01 01:42_

---

_Closed by @dhruvmanila on 2023-12-01 01:42_

---

_Branch deleted on 2023-12-01 01:42_

---

_Comment by @github-actions[bot] on 2023-12-01 01:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:106 on 2023-12-01 01:57_

Will this range be incorrect if the user uses a keyword argument?

---

_@charliermarsh reviewed on 2023-12-01 01:57_

---

_@charliermarsh reviewed on 2023-12-01 01:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:106 on 2023-12-01 01:57_

We probably want "replace call.arguments.range with ()".

---

_@dhruvmanila reviewed on 2023-12-01 02:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:106 on 2023-12-01 02:14_

I'm out now but I think that should still work as `arg` is either a positional or keyword argument.

---

_@charliermarsh reviewed on 2023-12-01 02:17_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:106 on 2023-12-01 02:17_

But `arg` is just the value, I think.

---

_@charliermarsh reviewed on 2023-12-01 03:03_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/zero_sleep_call.rs`:106 on 2023-12-01 03:03_

Fixed in https://github.com/astral-sh/ruff/pull/8936.

---
