```yaml
number: 20224
title: "[`flake8-logging-format`] handles grouping parens and preserves r-prefix/quotes"
type: pull_request
state: open
author: TaKO8Ki
labels: []
assignees: []
base: main
head: fix-handles-grouping-parens-and-preserves-r-prefix-quotes
created_at: 2025-09-04T08:24:01Z
updated_at: 2025-11-21T07:57:36Z
url: https://github.com/astral-sh/ruff/pull/20224
synced_at: 2026-01-10T17:34:34Z
```

# [`flake8-logging-format`] handles grouping parens and preserves r-prefix/quotes

---

_Pull request opened by @TaKO8Ki on 2025-09-04 08:24_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes some parts of #20151

Handles grouping parens and preserves r-prefix/quotes

- Iterate over `FStringPart` to build the format string, handling implicit string literal concatenation.
- Preserve the original quote style and `r` prefix in the generated message string.
- Replace over the expression’s parenthesized range to avoid tuple args, e.g. `logging.warning((f"{Ellipsis}"))` -> `logging.warning("%s", Ellipsis)`.
- Update tests and snapshots for nested parentheses and backslash escape case.

## Test Plan

<!-- How was it tested? -->

I have updated `crates/ruff_linter/resources/test/fixtures/flake8_logging_format/G004.py`.


---

_Comment by @github-actions[bot] on 2025-09-04 08:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-09 12:59_

---

_Renamed from "[`flake8-logging-format`] fix handles grouping parens and preserves r-prefix/quotes" to "[`flake8-logging-format`] handles grouping parens and preserves r-prefix/quotes" by @TaKO8Ki on 2025-09-10 16:59_

---

_Comment by @TaKO8Ki on 2025-09-30 02:16_

@dylwil3 I have resolve the conflicts.

---

_Comment by @TaKO8Ki on 2025-10-17 18:49_

@dylwil3 @ntBre Gentle reminder: Could someone review this pull request?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:42 on 2025-10-18 21:00_

Is this right? What if, in an implicitly concatenated string, the first part is not raw but later parts are? Should we be keeping the string implicitly concatenated?

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_logging_format/G004.py`:71 on 2025-10-18 21:08_

Can we add some more exotic tests? e.g. ones where...

1. The raw prefix is on a different implicitly concatenated part?
2. Some of the weird behavior we're checking for lie in a different concatenated part?
3. There are more grouping parentheses

---

_@dylwil3 requested changes on 2025-10-18 21:09_

Thanks for working on this and apologies for the delay! I'd like to see a little more test coverage and I'm not confident we've solved the puzzle of implicitly concatenated strings correctly yet. If it becomes too complicated, another option would be to not offer the fix in the presence of implicit concatenation.

---

_Comment by @MichaReiser on 2025-11-21 07:57_

@TaKO8Ki I just wanted to check in with you if you plan on revisiting this PR. Please don't feel pressured to do so, I'm only asking to understand what the next steps are on this PR.

---
