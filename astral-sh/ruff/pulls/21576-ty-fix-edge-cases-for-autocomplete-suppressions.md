```yaml
number: 21576
title: "[ty] Fix edge cases for autocomplete suppressions in variable bindings"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: improve-variable-binding-suggestions
created_at: 2025-11-22T12:22:51Z
updated_at: 2025-11-25T14:09:38Z
url: https://github.com/astral-sh/ruff/pull/21576
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix edge cases for autocomplete suppressions in variable bindings

---

_Pull request opened by @RasmusNygren on 2025-11-22 12:22_

## Summary
This handles a few edge cases related to https://github.com/astral-sh/ty/issues/1563  that was not fully handled in https://github.com/astral-sh/ruff/pull/21549

Only using the individual `Parameter` nodes in the AST to determine parameter bindings is not enough when you're in a state where the syntax is invalid. For example when the name of the parameter clashes with a keyword. In those scenarios we now go up a level and inspect the `Parameters` node to determine if we're still binding variables.


## Test Plan

New tests and ty-playground sanity-check.


---

_Review requested from @carljm by @RasmusNygren on 2025-11-22 12:22_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-11-22 12:22_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-11-22 12:22_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-11-22 12:22_

---

_Review requested from @dcreager by @RasmusNygren on 2025-11-22 12:22_

---

_Label `server` added by @AlexWaygood on 2025-11-22 12:23_

---

_Label `ty` added by @AlexWaygood on 2025-11-22 12:23_

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 12:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-22 12:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1355 on 2025-11-22 12:26_

This is technically not relevant to this PR but I realised from https://github.com/astral-sh/ruff/pull/21570 that this calculation was logically incorrect. `typed.textlen()` is the length of the entire token, even if the cursor is currently in the middle of the token which is not quite what we want.
 
 I have not managed to write a test based on suggestions that highlights this bug, but by calling `is_in_variable_binding` directly with different inputs it became obvious that it was previously incorrect.

---

_@RasmusNygren reviewed on 2025-11-22 12:26_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-11-22 12:27_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-22 14:01_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-22 14:01_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-22 14:01_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-22 14:01_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1349 on 2025-11-24 20:05_

This actually wasn't my intent, but maybe I was wrong. In particular, rust-analyzer and pylance work how you describe: it only considers the characters before the cursor when the cursor splits a token. ty doesn't. It uses the entire token as the query.

I initially went this route because when you select a completion, clients seem to usually replace the _entire_ token with the selected suggestion. So in my mind, it made sense for the query to also correspond to the entire token. With that said, while rust-analyzer replaces the entire token, pylance does not.

I do kind of feel like only taking the text before the cursor is perhaps more intuitive and even more useful. (I'm undecided on the token replacement strategy though.)

So to that end, perhaps the code that extracts the typed text should cut itself off based on the offset? Then I think that would fix this without needing to do this extra arithmetic here.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1427 on 2025-11-24 20:11_

This makes sense to me. Nice find!

---

_@BurntSushi reviewed on 2025-11-24 20:11_

---

_@RasmusNygren reviewed on 2025-11-24 21:13_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1349 on 2025-11-24 21:13_

Right, I agree with you that only considering text to the left of the cursor feels more intuitive, independently from whether the client replaces the entire token or not. Given that, it would be quite nice to only extract the relevant bit from the start, so we can be consistent without any special handling.

Would you like me to open a separate PR (maybe even an issue) for that (as I jumbled two issues into one PR here) or are you fine if I execute on that directly here?


---

_@BurntSushi reviewed on 2025-11-24 21:15_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1349 on 2025-11-24 21:15_

I think just doing that directly here is fine. :-) Thank you!

---

_Review requested from @BurntSushi by @RasmusNygren on 2025-11-24 21:45_

---

_@BurntSushi approved on 2025-11-25 13:25_

Beautiful. I also added a test that `foo<CURSOR>bar` only takes `foo` into account when offering suggestions.

Before:

https://github.com/user-attachments/assets/a771f855-f54e-41ae-93a4-8b69b2a56eee

After:

https://github.com/user-attachments/assets/641f2f34-12ef-46f9-9ca0-3ee8cddc758d

---

_Merged by @BurntSushi on 2025-11-25 14:09_

---

_Closed by @BurntSushi on 2025-11-25 14:09_

---
