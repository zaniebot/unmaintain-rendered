```yaml
number: 18805
title: "Handle parenthesized arguments in `remove_argument`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-RUF056
created_at: 2025-06-19T21:51:06Z
updated_at: 2025-06-22T13:46:10Z
url: https://github.com/astral-sh/ruff/pull/18805
synced_at: 2026-01-10T18:39:09Z
```

# Handle parenthesized arguments in `remove_argument`

---

_Pull request opened by @LaBatata101 on 2025-06-19 21:51_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fix `remove_argument` to handle parenthesized arguments properly. Previously, it would not delete the entire parenthesized argument, leaving the close parenthesis. This caused the autofix of some rules to introduce a syntax error, like in the case of #18798.

 I had to update the function signature to include the `CommentRanges`, that's why there are some many files changed.

Fixes #18798
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @LaBatata101 on 2025-06-19 21:51_

---

_Comment by @github-actions[bot] on 2025-06-19 21:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @AlexWaygood on 2025-06-20 07:47_

---

_Label `fixes` added by @AlexWaygood on 2025-06-20 07:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:254 on 2025-06-20 10:30_

this looks like the correct fix, but we need to apply the fix more holistically -- the same bug is present in the other branches of this `match`. Your current PR only fixes `remove_argument` if the argument being removed is the _last_ argument.

For example, on your branch:

```
(experiment) ~/dev/ruff (fix-RUF056)⚡ % cat foo.py
range((0), 42)
(experiment) ~/dev/ruff (fix-RUF056)⚡ % cargo run -- check foo.py --no-cache --preview --select=PIE808 --diff
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.36s
     Running `target/debug/ruff check foo.py --no-cache --preview --select=PIE808 --diff`
error: Fix introduced a syntax error in `foo.py` with rule codes PIE808: unexpected EOF while parsing at byte range 11..11
---
range((42)

---
```

---

_@AlexWaygood requested changes on 2025-06-20 10:31_

Thanks!

---

_@LaBatata101 reviewed on 2025-06-20 14:57_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/fix/edits.rs`:254 on 2025-06-20 14:57_

Yeah, I knew that. I was expecting one of the reviewers to point out a rule that would trigger the other branch so that I could test it 🙂

---

_Review requested from @AlexWaygood by @LaBatata101 on 2025-06-20 15:12_

---

_@AlexWaygood reviewed on 2025-06-20 15:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:254 on 2025-06-20 15:26_

can you add `range((0), 42)` as a test case to the `PIE808` fixtures?

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/fix/edits.rs`:254 on 2025-06-20 19:52_

Done!

---

_@LaBatata101 reviewed on 2025-06-20 19:52_

---

_@AlexWaygood approved on 2025-06-20 21:22_

Thanks!

---

_Merged by @AlexWaygood on 2025-06-20 21:24_

---

_Closed by @AlexWaygood on 2025-06-20 21:24_

---

_Branch deleted on 2025-06-22 13:46_

---
