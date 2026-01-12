```yaml
number: 18867
title: "[`flake8-errmsg`] Extend `EM101` to support byte strings"
type: pull_request
state: merged
author: njhearp
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: EM101-byte-string
created_at: 2025-06-22T21:46:08Z
updated_at: 2025-06-25T14:53:56Z
url: https://github.com/astral-sh/ruff/pull/18867
synced_at: 2026-01-12T15:56:26Z
```

# [`flake8-errmsg`] Extend `EM101` to support byte strings

---

_@njhearp_

## Summary

Fixes #18765

## Test Plan

Added test


---

_Renamed from "Extended `EM101` to support byte strings" to "[`flake8-errmsg`] Extended `EM101` to support byte strings" by @njhearp on 2025-06-22 21:48_

---

_Comment by @github-actions[bot] on 2025-06-22 22:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @njhearp on 2025-06-23 02:36_

Following the recommendation from the error got the CI tests to pass. I'm just a little concerned about the initial failure.

---

_Comment by @MichaReiser on 2025-06-23 06:44_

> Following the recommendation from the error got the CI tests to pass. I'm just a little concerned about the initial failure.

What was the initial failure?

---

_Label `rule` added by @MichaReiser on 2025-06-23 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs`:208 on 2025-06-23 06:45_

```suggestion
                    if checker.is_rule_enabled(Rule::RawStringInException) {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs`:208 on 2025-06-23 06:48_

I think we should gate this change behind preview as it is an extension of a stable rule

See https://github.com/astral-sh/ruff/blob/daa385c1a991cc87c77642eccdcc2a888d3586a0/crates/ruff_linter/src/preview.rs#L60 for other cases where we gated behavior behind a preview flag.

---

_@MichaReiser reviewed on 2025-06-23 06:48_

---

_Comment by @njhearp on 2025-06-23 11:33_

```
error[E0599]: no method named `enabled` found for reference `&Checker<'_>` in the current scope
   --> crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs:208:32
    |
208 |                     if checker.enabled(Rule::RawStringInException) {
    |                                ^^^^^^^ method not found in `&Checker<'_>`
    |
    = help: items from traits can only be used if the trait is implemented and in scope
    = note: the following traits define an item `enabled`, perhaps you need to implement one of them:
            candidate #1: `Log`
            candidate #2: `tracing_core::subscriber::Subscriber`
            candidate #3: `tracing_subscriber::layer::Filter`
            candidate #4: `tracing_subscriber::layer::Layer`
help: one of the expressions' fields has a method of the same name
    |
208 |                     if checker.settings.rules.enabled(Rule::RawStringInException) {
    |                                +++++++++++++++
```
The CI error suggested this. I believe it is because of Rust's ownership; it only flagged line 208. I just wanted to make sure there was no problem with it.

---

_Comment by @ntBre on 2025-06-23 12:05_

You may just need to rebase or merge `main`. We recently renamed `Checker::enabled` to `Checker::is_rule_enabled` and also made the `settings` field a method.

---

_@ntBre reviewed on 2025-06-23 12:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_errmsg/rules/string_in_exception.rs`:209 on 2025-06-23 12:07_

```suggestion
                        if bytes.len() >= checker.settings().flake8_errmsg.max_string_length {
```

---

_Review requested from @carljm by @njhearp on 2025-06-24 04:33_

---

_Review requested from @AlexWaygood by @njhearp on 2025-06-24 04:33_

---

_Review requested from @sharkdp by @njhearp on 2025-06-24 04:33_

---

_Review requested from @dcreager by @njhearp on 2025-06-24 04:33_

---

_Review request for @dcreager removed by @carljm on 2025-06-24 04:36_

---

_Review request for @carljm removed by @carljm on 2025-06-24 04:36_

---

_Review request for @sharkdp removed by @carljm on 2025-06-24 04:36_

---

_Comment by @github-actions[bot] on 2025-06-24 04:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @njhearp on 2025-06-24 04:59_

I updated it so the change is gated behind the preview flag. Also, I merged with main. Sorry about the mess with the history; I realized it would have been cleaner to rebase.

---

_Comment by @MichaReiser on 2025-06-24 06:29_

@njhearp I think something went wrong with your merge. The diff contains too many unrelated changes (which should already be on main?). I'm not quite sure what went wrong here. Could you try doing an interactive rebase and only pick your commits?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-24 07:31_

---

_Review requested from @ntBre by @MichaReiser on 2025-06-25 06:56_

---

_Label `preview` added by @ntBre on 2025-06-25 14:47_

---

_@ntBre approved on 2025-06-25 14:53_

Looks great, thank you!

---

_Renamed from "[`flake8-errmsg`] Extended `EM101` to support byte strings" to "[`flake8-errmsg`] Extend `EM101` to support byte strings" by @ntBre on 2025-06-25 14:53_

---

_Merged by @ntBre on 2025-06-25 14:53_

---

_Closed by @ntBre on 2025-06-25 14:53_

---
