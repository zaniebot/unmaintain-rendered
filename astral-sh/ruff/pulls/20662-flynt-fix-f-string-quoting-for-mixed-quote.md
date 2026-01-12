```yaml
number: 20662
title: "[`flynt`] Fix f-string quoting for mixed quote joiners (`FLY002`)"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-flynt-mixed-quote-join
created_at: 2025-10-01T06:21:07Z
updated_at: 2025-10-03T13:16:11Z
url: https://github.com/astral-sh/ruff/pull/20662
synced_at: 2026-01-12T15:57:07Z
```

# [`flynt`] Fix f-string quoting for mixed quote joiners (`FLY002`)

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19837

Track quote usage across the joiner and parts to choose a safe f-string quote or skip the fix when both appear.

## Test Plan

<!-- How was it tested? -->

Add regression coverage to FLY002.py


---

_Renamed from "['ruff'] Fix flynt f-string quoting for mixed quote joiners" to "[`ruff`] Fix flynt f-string quoting for mixed quote joiners" by @TaKO8Ki on 2025-10-01 06:21_

---

_@TaKO8Ki reviewed on 2025-10-01 06:22_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:147 on 2025-10-01 06:22_

If the content includes both single quote and double quote, ruff skips FLY002 for now.

I will improve this logic in a separate pull request as we discussed here.

https://github.com/astral-sh/ruff/pull/20197#pullrequestreview-3240917942

---

_Comment by @github-actions[bot] on 2025-10-01 06:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@amyreese approved on 2025-10-03 00:15_

---

_Review requested from @ntBre by @MichaReiser on 2025-10-03 07:41_

---

_@ntBre approved on 2025-10-03 13:15_

Thank you!

---

_Renamed from "[`ruff`] Fix flynt f-string quoting for mixed quote joiners" to "[`flynt`] Fix f-string quoting for mixed quote joiners (`FLY002`)" by @ntBre on 2025-10-03 13:15_

---

_Label `bug` added by @ntBre on 2025-10-03 13:15_

---

_Label `fixes` added by @ntBre on 2025-10-03 13:15_

---

_Merged by @ntBre on 2025-10-03 13:15_

---

_Closed by @ntBre on 2025-10-03 13:15_

---

_Branch deleted on 2025-10-03 13:16_

---
