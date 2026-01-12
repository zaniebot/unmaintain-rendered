```yaml
number: 18845
title: "Remove redundant `settings` field from  `Checker`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - internal
assignees: []
merged: true
base: main
head: beep-bop
created_at: 2025-06-21T02:21:32Z
updated_at: 2025-06-23T13:15:10Z
url: https://github.com/astral-sh/ruff/pull/18845
synced_at: 2026-01-12T15:56:26Z
```

# Remove redundant `settings` field from  `Checker`

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR removes the redundant `settings` field from `Checker` in favor of the `Checker::context.settings`.

Closes [#18828](https://github.com/astral-sh/ruff/issues/18828)
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "Use the LinterSettings on LintContext instead of Checker" to "Use the `LinterSettings` on `LintContext` instead of `Checker`" by @LaBatata101 on 2025-06-21 02:22_

---

_Comment by @ntBre on 2025-06-21 16:29_

I think the new lifetime parameter and the `settings` method were bad suggestions from me that really complicate things and might even be impossible. Sorry about that. There's definitely at least one place where a `settings` method can't be used because it borrows the whole `Checker` instead of just the field like the field access. I think that may be the problem here too because I ran into this same group of functions even without the new lifetime parameter.

The easiest fix of just replacing every `checker.settings` access with `checker.context.settings` and making those fields `pub(crate)` might be the way to go.

---

_Comment by @MichaReiser on 2025-06-21 16:31_

> The easiest fix of just replacing every checker.settings access with checker.context.settings and making those fields pub(crate) might be the way to go.

Can you go into more details? I would really prefer if we could lock down some of the `Checker` fields. Giving direct access has complicated refactors like this in the past (and do now) because moving the field always requires updating all uses

---

_Comment by @MichaReiser on 2025-06-21 16:44_

I reverted the liftime changes and changed `settings` to return `&'a LinterSettings`. This fixes all lifetime problems

---

_Comment by @ntBre on 2025-06-21 16:45_

Amazing, thanks Micha. Obviously I did over-complicate things but not in the way I thought. I also thought I tried that but clearly I did not.

---

_Comment by @github-actions[bot] on 2025-06-21 17:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Use the `LinterSettings` on `LintContext` instead of `Checker`" to "Remove redundant `settings` field from  `Checker`" by @LaBatata101 on 2025-06-21 20:46_

---

_Marked ready for review by @LaBatata101 on 2025-06-21 20:49_

---

_Review requested from @AlexWaygood by @LaBatata101 on 2025-06-21 20:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-22 12:17_

---

_Label `internal` added by @MichaReiser on 2025-06-23 09:06_

---

_@MichaReiser approved on 2025-06-23 09:06_

Thank you

---

_Merged by @MichaReiser on 2025-06-23 09:06_

---

_Closed by @MichaReiser on 2025-06-23 09:06_

---

_Branch deleted on 2025-06-23 13:15_

---
