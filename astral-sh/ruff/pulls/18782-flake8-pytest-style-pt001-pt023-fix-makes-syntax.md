```yaml
number: 18782
title: "[`flake8-pytest-style`] PT001/PT023 fix makes syntax error on parenthesized decorator"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-18771
created_at: 2025-06-19T05:28:28Z
updated_at: 2025-06-23T12:46:25Z
url: https://github.com/astral-sh/ruff/pull/18782
synced_at: 2026-01-12T15:56:25Z
```

# [`flake8-pytest-style`] PT001/PT023 fix makes syntax error on parenthesized decorator

---

_@danparizher_

Fixes https://github.com/astral-sh/ruff/issues/18771

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

1. PT001 – `@pytest.fixture` parentheses style
- Handles alias-imported fixtures and decorators wrapped in an outer pair of parentheses (e.g. @(pytest.fixture())).
- Ensures the rule honours the `lint.flake8-pytest-style.fixture-parentheses` setting for every variant.
- Updates reference tests (`PT001.py`) and snapshots.

2. PT023 – `@pytest.mark.<marker>` parentheses style
- Extends the checker so it also flags/removes superfluous parentheses when the mark is applied to classes, nested classes, or is itself wrapped in outer parentheses.
- Keeps behaviour consistent with `lint.flake8-pytest-style.mark-parentheses`.
- Adds/updates fixture file (`PT023.py`) and snapshots.


## Test Plan

1. Unit / snapshot tests
- `cargo nextest run` (or `cargo test`) — all suites pass.
- Updated `PT001` and `PT023` snapshots reviewed and accepted via cargo insta review.

2. Manual smoke test
- Ran `ruff check` on a sample project with mixed fixture/mark styles to confirm that:
- Unnecessary parentheses are now flagged/auto-fixed in every location.
- Legitimate calls with arguments remain unaffected.

---

_Comment by @github-actions[bot] on 2025-06-19 05:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-06-23 11:31_

Thank you

---

_Label `bug` added by @MichaReiser on 2025-06-23 11:42_

---

_Label `fixes` added by @MichaReiser on 2025-06-23 11:42_

---

_Merged by @MichaReiser on 2025-06-23 11:46_

---

_Closed by @MichaReiser on 2025-06-23 11:46_

---

_Branch deleted on 2025-06-23 12:46_

---
