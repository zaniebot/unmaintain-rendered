```yaml
number: 21853
title: "[pylint] Implement too-many-lines (C0302)"
type: pull_request
state: open
author: tluolamo
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: feat/implement-max-file-length-rule
created_at: 2025-12-08T21:13:23Z
updated_at: 2025-12-08T22:52:10Z
url: https://github.com/astral-sh/ruff/pull/21853
synced_at: 2026-01-10T16:42:11Z
```

# [pylint] Implement too-many-lines (C0302)

---

_Pull request opened by @tluolamo on 2025-12-08 21:13_

## Summary

- Add Pylint `too-many-lines` (C0302) as a preview physical-lines rule with a 1000-line default, configurable via `lint.pylint.max-module-lines`.
- Wire the rule into the linter registry/checker, remove the old flake8 file-length plumbing, and expose the new setting in workspace options and schema.
- Add inline rule tests and regenerate docs/schema.

## Test Plan

- `cargo test -p ruff_linter --lib`
- `cargo dev generate-all`


---

_Comment by @ntBre on 2025-12-08 22:52_

Thanks for working on this! However, I see that this rule is crossed out in the summary of #970 with the note "not compatible with the formatter," so I don't think we want to add this rule.

---

_Label `rule` added by @ntBre on 2025-12-08 22:52_

---

_Label `needs-decision` added by @ntBre on 2025-12-08 22:52_

---
