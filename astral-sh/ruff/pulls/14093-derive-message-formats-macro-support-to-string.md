```yaml
number: 14093
title: Derive message formats macro support to string
type: pull_request
state: merged
author: sbrugman
labels:
  - internal
assignees: []
merged: true
base: main
head: derive-message-formats-macro-support-to-string
created_at: 2024-11-04T15:11:49Z
updated_at: 2024-11-04T17:09:25Z
url: https://github.com/astral-sh/ruff/pull/14093
synced_at: 2026-01-12T15:55:46Z
```

# Derive message formats macro support to string

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow-up of https://github.com/astral-sh/ruff/pull/14090: the `derive_message_formats` macro previously did not support `String::to_string`. This PR changes the macro to require `String::to_string` instead of `format!` when no formatting parameters are provided. 

There is room for further expansion of this macro, as this pattern used in #14090 is not yet supported for the message method:

```rust
let message = "message";
message.to_string();
```

## Test Plan

`cargo test` and `cargo expand`, e.g. `cargo expand rules::flake8_bandit::rules::jinja2_autoescape_false -p ruff_linter`


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-04 15:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:62 on 2024-11-04 15:23_

Here I would probably keep the `let NeedlessBool { condition, negate } = self;` line, since it results in fewer lines of code overall, and it allows us to unpack multiple variables at once

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/zip_dict_keys_and_values.rs`:55 on 2024-11-04 15:25_

Same here, the unpacking of `self` was fine and resulted in fewer lines of code overall. I'd keep the unpacking as it was in these places.

---

_Comment by @github-actions[bot] on 2024-11-04 15:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:69 on 2024-11-04 15:28_

I'd prefer to keep the `let BlanketNoQA { .. } = self;` line for situations where we're unpacking lots of variables at once, like this

---

_@AlexWaygood reviewed on 2024-11-04 15:32_

Very nice! Just three places where I think you went _slightly_ too far ;)

---

_Comment by @sbrugman on 2024-11-04 15:45_

Fair point, reverted those three cases

---

_@AlexWaygood approved on 2024-11-04 15:49_

LGTM. It might be worth @MichaReiser -- or somebody else who knows their Rust macros a little better -- also taking a look at the changes in `ruff_macros`, though 😄

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-04 15:51_

---

_Label `internal` added by @MichaReiser on 2024-11-04 16:57_

---

_@MichaReiser approved on 2024-11-04 17:06_

Hmm, I don't mind the change and it certainly helps that people don't learn the "bad" pattern of using `format! over `to_string` if there are no arguments. 

At the other hand, `to_string` calls `Display`. So `format!` and `to_string` are equivalent. So this is a pure "esthetic" change. I'm okay with it because it doesn't add too much complexity to the macros and my main concern is with the macro itself (but that's a whole other story). 

---

_Merged by @MichaReiser on 2024-11-04 17:06_

---

_Closed by @MichaReiser on 2024-11-04 17:06_

---

_Branch deleted on 2024-11-04 17:09_

---
