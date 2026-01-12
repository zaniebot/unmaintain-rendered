```yaml
number: 18329
title: "[ty] Check shifts of `Literal` `int`s"
type: pull_request
state: closed
author: brandtbucher
labels:
  - ty
assignees: []
draft: true
base: main
head: shifts
created_at: 2025-05-27T03:05:46Z
updated_at: 2025-07-10T14:11:37Z
url: https://github.com/astral-sh/ruff/pull/18329
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Check shifts of `Literal` `int`s

---

_@brandtbucher_

Fixes astral-sh/ty#517.

I took the liberty of adding a new `negative-shift` diagnostic for cases when we detect shifting by a negative RHS, which is a `ValueError` at runtime. This reuses some machinery that's already used to detect division by `Literal[0]` (`division-by-zero`), which felt sort of natural. I'm okay with refactoring or removing it, though... I was mostly just curious how new rules are added and used under-the-hood.

---

_Review requested from @carljm by @brandtbucher on 2025-05-27 03:05_

---

_Review requested from @AlexWaygood by @brandtbucher on 2025-05-27 03:05_

---

_Review requested from @sharkdp by @brandtbucher on 2025-05-27 03:05_

---

_Review requested from @dcreager by @brandtbucher on 2025-05-27 03:05_

---

_Review requested from @MichaReiser by @brandtbucher on 2025-05-27 03:05_

---

_Renamed from "Check shifts of literals ints" to "[ty] Check shifts of `Literal` `int`s" by @brandtbucher on 2025-05-27 03:06_

---

_Comment by @brandtbucher on 2025-05-27 03:29_

That was neat: `mypy_primer` failed because handling `Literal` shifts shook out an unrelated bug where negating `Literal[-9223372036854775808]` (`i64::MIN`) would panic due to overflow. I've fixed that in this PR as well, and added a regression test.

---

_Comment by @github-actions[bot] on 2025-05-27 03:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @brandtbucher on 2025-05-27 03:29_

Yay Rust.

---

_Comment by @brandtbucher on 2025-05-27 05:44_

Just realized that this doesn't correctly handle the cases where we shift a `1` into the sign bit (like `1 << 63`). I'll fix that tomorrow.

---

_Converted to draft by @MichaReiser on 2025-05-27 05:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/integers.md`:1 on 2025-05-27 10:44_

Could you also add some tests with boolean literals? E.g. `True << 42`, `56 >> False` and `True << True`. Your branch gives the right answer for them, but it's good to explicitly test them, I think.

(They're covered by this fallback branch here: https://github.com/astral-sh/ruff/blob/8d5655a7baa1ecc84a906cbfc0e2294cf173dee7/crates/ty_python_semantic/src/types/infer.rs#L6244-L6257)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:958 on 2025-05-27 10:54_

I wonder if it's worth this being a separate lint to `DIVISION_BY_ZERO`, or if it might be better to group them both under a `LITERAL_MATH_ERROR` lint (which, like `DIVISION_BY_ZERO`, should probably be disabled by default for now):
- They're conceptually pretty similar in the kind of error they detect
- I can't really think of a reason why a user would want to suppress one (either on a per-line, per-file or per-project basis) but not the other
- I think they both suffer from the same drawbacks that @carljm noted in the PR description for https://github.com/astral-sh/ruff/pull/18220#issue-3077312707

Renaming the existing `DIVISION_BY_ZERO` lint might be a fair amount of busywork, though, so it's okay if you're not up to that! I think one of us could reasonably take on merging the two lints as a followup task. If you do keep this as a separate lint, though, I think it should _probably_ be disabled by default, similar to `DIVISION_BY_ZERO`. (See https://github.com/astral-sh/ruff/pull/18220 for what the necessary changes there would look like!)

---

_@AlexWaygood reviewed on 2025-05-27 10:54_

Nice!

---

_Label `ty` added by @AlexWaygood on 2025-05-27 13:42_

---

_Comment by @MichaReiser on 2025-07-10 14:11_

Thanks for your contribution. I'll close this as it seems to have gone stale. Anyone interested in picking this up. Feel free to open a new PR which addresses the feedback.

---

_Closed by @MichaReiser on 2025-07-10 14:11_

---
