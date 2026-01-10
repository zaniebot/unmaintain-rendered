```yaml
number: 22198
title: Document the options used by more rules
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/document-options
created_at: 2025-12-25T22:56:40Z
updated_at: 2025-12-26T16:24:51Z
url: https://github.com/astral-sh/ruff/pull/22198
synced_at: 2026-01-10T16:36:18Z
```

# Document the options used by more rules

---

_Pull request opened by @ntBre on 2025-12-25 22:56_

Summary
--

While analyzing our rules, I wanted to know which of them use configuration options but noticed that some of them were not documented (or at least not documented in a separate `## Options` section).

I had Claude generate an initial list of candidate rules, but it contained a lot of false positives that I filtered out, and I ended up adding all of these sections myself. I'm not claiming that the options lists are exhaustive (as in the rules may use additional options beyond what I found), but this will at least help with my goal of determining whether or not a rule is configurable at all and also hopefully be helpful in general.

I mostly just tacked on an `## Options` section without any commentary, but I added a couple lines of explanation when I felt that the meaning of the options wasn't obvious from the context.

I also noticed a bit of variation in the `flake8-simplify` rules from doing this. Some of them offer a diagnostic but no fix depending on the resulting line length of the suggestion, while others offer neither. I'm not sure we need to do anything different here, but it seemed worth mentioning.

Test Plan
--

Docs tests to make sure the links are right


---

_Label `documentation` added by @ntBre on 2025-12-25 22:56_

---

_Comment by @astral-sh-bot[bot] on 2025-12-25 23:05_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @ntBre on 2025-12-25 23:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/cached_instance_method.rs`:63 on 2025-12-26 08:23_

It wasn't immediately clear to me why this is relevant for the listed options. Maybe say something that: You can use the following options to customize how Ruff classifies methods

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_except_handler.rs`:49 on 2025-12-26 08:25_

You sometimes added a blankline after the last option and sometimes you didn't. I don't bother that it isn't consistent. I just want to ensure the empty line isn't required for the docs to render properly.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/singledispatch_method.rs`:51 on 2025-12-26 08:26_

Do we need a blank line before the options (here and in a few other places below)?

Maybe add the docs about customizing how Ruff classifies methods 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/singledispatchmethod_function.rs`:45 on 2025-12-26 08:26_

Same here

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/super_without_brackets.rs`:55 on 2025-12-26 08:26_

and here

---

_@MichaReiser approved on 2025-12-26 08:27_

Awesome, thank you

---

_@ntBre reviewed on 2025-12-26 15:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_except_handler.rs`:49 on 2025-12-26 15:22_

I only (intentionally) added a blank line if there were additional sections after the options, usually `## References`. I'll double check and also build the docs locally to make sure everything looks okay.

---

_@ntBre reviewed on 2025-12-26 15:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/singledispatchmethod_function.rs`:45 on 2025-12-26 15:28_

On second thought, I might actually remove this one. The rule does technically use them by passing them to `FunctionType::classify`, but they should effectively be ignored since the rule only applies to functions defined outside of a class scope.

---

_@ntBre reviewed on 2025-12-26 16:09_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_except_handler.rs`:49 on 2025-12-26 16:09_

These seem to be rendering correctly!

<img width="654" height="284" alt="image" src="https://github.com/user-attachments/assets/fe27264e-21ad-40cc-8b19-a0b7e889e765" />


---

_Renamed from "Document more rules with options" to "Document the options used by more rules" by @ntBre on 2025-12-26 16:24_

---

_Merged by @ntBre on 2025-12-26 16:24_

---

_Closed by @ntBre on 2025-12-26 16:24_

---

_Branch deleted on 2025-12-26 16:24_

---
