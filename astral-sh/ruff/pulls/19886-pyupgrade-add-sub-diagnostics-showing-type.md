```yaml
number: 19886
title: "[`pyupgrade`] Add sub-diagnostics showing type variable definitions (`UP046`)"
type: pull_request
state: closed
author: ntBre
labels:
  - diagnostics
assignees: []
draft: true
base: main
head: brent/up046-subdiagnostics
created_at: 2025-08-12T21:40:34Z
updated_at: 2025-08-25T17:57:01Z
url: https://github.com/astral-sh/ruff/pull/19886
synced_at: 2026-01-12T15:56:49Z
```

# [`pyupgrade`] Add sub-diagnostics showing type variable definitions (`UP046`)

---

_@ntBre_

Summary
--

Now that we've handled caching for the new diagnostics, I went looking for diagnostics that could use additional annotations or sub-diagnostics that weren't already tracked in #17203. UP046 (and UP040 and UP047 if we like this one) seemed like a reasonable candidate because we check for separate type variable definitions in the same file before emitting a diagnostic. In this case, I added an info-level sub-diagnostic pointing to this type variable definition. I'm not actually sure if this is super helpful information, so I wouldn't mind closing this if others don't think it's useful.

I also added a helper `DiagnosticGuard::info` method that I think will be useful for adding this kind of sub-diagnostic in general.

I spent a while debugging why the snapshots were showing a longer path in the sub-diagnostics before remembering that we overwrite the source file for the main diagnostic here:

https://github.com/astral-sh/ruff/blob/79c949f0f75f6582782f3d5fddc65089789801d5/crates/ruff_linter/src/test.rs#L280-L288

As shown below, the sub-diagnostics look as expected in the CLI. I think we _could_ delete this part of the test code, but it results in using the longer test file names (like our new sub-diagnostics) in every snapshot. So alternatively, we may want to apply the same transformation to the sub-diagnostics (and secondary annotations) too.

Test Plan
--

Existing UP046 tests with newly expanded snapshots, as well as some manual testing in the CLI:

<img width="748" height="328" alt="image" src="https://github.com/user-attachments/assets/f48a0d1b-8f93-4403-84a8-3aabd660cf8e" />

Alternative Idea
--

I don't really love the `help:` and `info:` lines stacked so closely together, so I also tried this as a secondary annotation:

<img width="683" height="264" alt="image" src="https://github.com/user-attachments/assets/d51ea0b6-bd0c-4931-ba1f-05ed0bd16277" />

That's not implemented in the PR but could be a different route with a small change to `DiagnosticGuard::info` (and a rename of the method).

---

_Comment by @github-actions[bot] on 2025-08-12 21:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-08-12 21:55_

Ah, I guess we need to replace all the paths to make the tests work on Windows anyway.

Also, I'm starting to think the secondary annotation might be a lot better, especially for multiple type variables:

<img width="672" height="295" alt="image" src="https://github.com/user-attachments/assets/0ef57679-0ffb-4b65-b56f-7b759a69e85b" />


---

_Comment by @MichaReiser on 2025-08-13 06:45_

> I don't really love the help: and info: lines stacked so closely together, so I also tried this as a secondary annotation:

I think what we do in ty is that help texts and notes are always the last sub-diagnostics. 



---

_Label `diagnostics` added by @AlexWaygood on 2025-08-13 13:46_

---

_Comment by @ntBre on 2025-08-13 13:59_

Ah, that does look better already. Thanks!

<img width="759" height="550" alt="image" src="https://github.com/user-attachments/assets/403d2d6c-b1bb-4831-b4c7-ca0e7fa582ba" />

I just did this by sorting the sub-diagnostics in our `DiagnosticGuard` `Drop` impl. That seemed like the easiest fix since we push the `help` diagnostic right when creating the `Diagnostic` from a `Violation`, but we might want a better solution longer-term.


---

_Comment by @ntBre on 2025-08-13 14:14_

I talked with Alex on Discord, and he echoed my concern that this wasn't a very useful piece of information to show to users. I'll go ahead and close this for now and hope to use some of the more general helper code for a different diagnostic.

---

_Closed by @ntBre on 2025-08-13 14:14_

---

_Branch deleted on 2025-08-25 17:57_

---
