```yaml
number: 15278
title: "[`pylint`] Fix `unreachable` infinite loop (`PLW0101`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - internal
assignees: []
merged: true
base: main
head: unreachable-fix
created_at: 2025-01-05T21:36:58Z
updated_at: 2025-01-09T04:49:02Z
url: https://github.com/astral-sh/ruff/pull/15278
synced_at: 2026-01-10T20:34:00Z
```

# [`pylint`] Fix `unreachable` infinite loop (`PLW0101`)

---

_Pull request opened by @augustelalande on 2025-01-05 21:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix infinite loop issue reported here #15248. Also re-make rule preview. The issue was caused by the break inside the if block, which caused the flow to exit in an unforeseen way. This caused other issues, eventually leading to an infinite loop.

As stated in #15252 we should do more fuzzing, however I'm not sure which scripts to run so please let me know.

Resolves #15248. Resolves #15336.

## Test Plan

Added failing code to fixture.


---

_Comment by @github-actions[bot] on 2025-01-05 21:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-01-06 02:39_

Would it be possible to add a test case that disambiguates this fix from the one in #15276? I believe both of these PRs produce the same CFG for the case in question.

---

_Label `bug` added by @MichaReiser on 2025-01-06 07:43_

---

_Label `preview` added by @MichaReiser on 2025-01-06 07:43_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-01-06 07:44_

---

_@dylwil3 requested changes on 2025-01-07 18:14_

This is great, thank you!

Let's keep the rule as test for a little longer, and it'd be nice to have add a snapshot that exhibits the new behavior for loops _without_ the presence of a try/finally block (if that's possible?)

Also you may need to rebase against main since I've merged in the PR that fixes the loop if there's a return in a `finally` block.

I think with those changes I'd be happy to merge!

---

_@MichaReiser approved on 2025-01-08 07:26_

---

_Comment by @MichaReiser on 2025-01-08 08:11_

I merged in main and reverted the changes that weren't necessary for the fix itself. The only thing that needs doing now is to add a test (if possible)

---

_@dylwil3 approved on 2025-01-08 15:02_

Added the latest example from the fuzzer, which passes. So let's merge this in and continue to test this rule and improve the CFG implementation.

Thanks so much for this fix - definitely making progress on this!

---

_Merged by @dylwil3 on 2025-01-08 15:45_

---

_Closed by @dylwil3 on 2025-01-08 15:45_

---

_Label `bug` removed by @MichaReiser on 2025-01-08 15:55_

---

_Label `preview` removed by @MichaReiser on 2025-01-08 15:55_

---

_Label `internal` added by @MichaReiser on 2025-01-08 15:55_

---

_Branch deleted on 2025-01-09 04:49_

---
