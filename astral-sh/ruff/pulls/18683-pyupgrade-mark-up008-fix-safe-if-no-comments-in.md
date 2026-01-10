```yaml
number: 18683
title: "[`pyupgrade`] Mark `UP008` fix safe if no comments in range"
type: pull_request
state: merged
author: robsdedude
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix/18533-up008-fix-safe-unless-comment-exists
created_at: 2025-06-15T11:45:54Z
updated_at: 2025-06-30T13:45:38Z
url: https://github.com/astral-sh/ruff/pull/18683
synced_at: 2026-01-10T18:39:08Z
```

# [`pyupgrade`] Mark `UP008` fix safe if no comments in range

---

_Pull request opened by @robsdedude on 2025-06-15 11:45_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Mark `UP008`'s fix safe if it won't delete comments.

## Relevant Issues
Fixes: https://github.com/astral-sh/ruff/issues/18533


---

_Comment by @github-actions[bot] on 2025-06-15 11:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-15 13:50_

---

_@ntBre reviewed on 2025-06-15 17:17_

Thanks! This looks right to me from a quick skim, but I think this will need to be gated behind preview since UP008 is a stable rule, as was done in https://github.com/astral-sh/ruff/pull/17644.

---

_Label `rule` added by @ntBre on 2025-06-15 17:17_

---

_Label `preview` added by @ntBre on 2025-06-15 17:17_

---

_Review requested from @ntBre by @robsdedude on 2025-06-15 19:09_

---

_Comment by @ntBre on 2025-06-16 19:11_

Could you resolve the merge conflicts here? Hopefully they're not too much trouble.

---

_Comment by @robsdedude on 2025-06-24 19:00_

@ntBre Done. Wasn't super straight-forward but manageable. Can you take a look so we can get it merged soon-ish before more conflicts arise? Thanks :bow: 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:50 on 2025-06-25 01:10_

I think I kind of preferred the old text for the first sentence/paragraph here. The second paragraph looks good, though.

I think you'll also need a reference link for `[preview]`, at least that's what I saw in other rules with a quick grep.

```suggestion
/// This rule's fix is marked as unsafe because removing the arguments from a call
/// may delete comments that are attached to the arguments.
///
/// In [preview], the fix is marked safe if no comments are present.
///
/// [preview]: https://docs.astral.sh/ruff/preview/
```

I guess we can do without the very last sentence if we bring back the old first bit.

---

_@ntBre approved on 2025-06-25 01:14_

Looks good, thanks! Just a couple of nits on the docs and another merge conflict, sorry!

---

_Review requested from @ntBre by @robsdedude on 2025-06-27 23:05_

---

_@ntBre approved on 2025-06-30 13:41_

---

_Merged by @ntBre on 2025-06-30 13:42_

---

_Closed by @ntBre on 2025-06-30 13:42_

---

_Branch deleted on 2025-06-30 13:45_

---
