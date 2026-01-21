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
updated_at: 2026-01-21T15:55:44Z
url: https://github.com/astral-sh/ruff/pull/21853
synced_at: 2026-01-21T16:04:58Z
```

# [pylint] Implement too-many-lines (C0302)

---

_@tluolamo_

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

_Comment by @devjerry0 on 2026-01-21 14:58_

+1 for merging this.
Resurfacing, running pylint standalone just for that rule is a tad annoying. 
The "not compatible with the formatter" rationale doesn't hold up:

The formatter changes line counts by maybe 1-5% through wrapping. If my limit is 200 lines, I don't care if the formatter makes it 195 or 205. The point is catching 500+ line monsters, not precise line counts.
You already ship PLR0915 (too-many-statements), which has the same "problem": the formatter can affect statement counts through expression splitting. Yet that rule exists and is useful.
This is blocking teams from consolidating on Ruff. Right now, I have to either:

Run Pylint alongside Ruff just for this one rule
Maintain a custom pre-commit script
Give up on enforcing file length
The implementation is done, and there's clear demand. Would love to see this merged, even if it stays in preview mode.


@tluolamo thx for your work on this!

---

_Comment by @ntBre on 2026-01-21 15:55_

Thanks for the feedback!

> You already ship PLR0915 (too-many-statements), which has the same "problem": the formatter can affect statement counts through expression splitting. Yet that rule exists and is useful.

I don't think this part is true. I'm fairly certain that the formatter shouldn't modify the AST when it's working correctly. If you've seen this happen, I'd love to see the code, and I think we'd consider it a bug!

(Not trying to invalidate the rest of the feedback, this part just caught my eye)


---
