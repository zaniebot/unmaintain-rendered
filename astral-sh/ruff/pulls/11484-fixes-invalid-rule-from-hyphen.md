```yaml
number: 11484
title: fixes invalid rule from hyphen
type: pull_request
state: merged
author: ekohilas
labels:
  - internal
assignees: []
merged: true
base: main
head: evan-fix-add-rule
created_at: 2024-05-21T19:35:29Z
updated_at: 2024-05-22T03:40:11Z
url: https://github.com/astral-sh/ruff/pull/11484
synced_at: 2026-01-12T15:55:38Z
```

# fixes invalid rule from hyphen

---

_@ekohilas_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When using `add_rule.py`, it produces the following line in `codes.rs`
```
        (Flake8Async, "102") => (RuleGroup::Stable, rules::flake8-async::rules::BlockingOsCallInAsyncFunction),
```

Causing a syntax error.

This PR resolves that issue so that the script can be used again.

## Test Plan

Tested manually in new rule creation


---

_Comment by @github-actions[bot] on 2024-05-21 19:54_

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

_Marked ready for review by @ekohilas on 2024-05-21 21:18_

---

_@charliermarsh approved on 2024-05-22 03:39_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-05-22 03:39_

---

_Label `documentation` removed by @charliermarsh on 2024-05-22 03:39_

---

_Label `internal` added by @charliermarsh on 2024-05-22 03:39_

---

_Merged by @charliermarsh on 2024-05-22 03:39_

---

_Closed by @charliermarsh on 2024-05-22 03:39_

---

_Comment by @charliermarsh on 2024-05-22 03:40_

I think there's high likelihood that there are other bugs and limitations in those scripts; they're not tested continuously so they tend to go stale.

---
