```yaml
number: 19910
title: "[ty] Default `ty.inlayHints.*` server settings to true"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - configuration
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/default-inlay-hints-true
created_at: 2025-08-14T08:09:45Z
updated_at: 2025-08-14T10:22:18Z
url: https://github.com/astral-sh/ruff/pull/19910
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Default `ty.inlayHints.*` server settings to true

---

_@dhruvmanila_

## Summary

This PR changes the default of `ty.inlayHints.*` settings to `true`.

I somehow missed this in my initial PR.

This is marked as `internal` because it's not yet released.

---

_Review requested from @carljm by @dhruvmanila on 2025-08-14 08:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-08-14 08:09_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-14 08:09_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-14 08:09_

---

_Label `internal` added by @dhruvmanila on 2025-08-14 08:09_

---

_Label `configuration` added by @dhruvmanila on 2025-08-14 08:09_

---

_Label `server` added by @dhruvmanila on 2025-08-14 08:09_

---

_Label `ty` added by @dhruvmanila on 2025-08-14 08:09_

---

_Comment by @github-actions[bot] on 2025-08-14 08:11_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 08:33:10.270535018 +0000
+++ new-output.txt	2025-08-14 08:33:10.336535119 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18ab)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(48c8)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-14 08:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-14 08:26_

---

_Merged by @dhruvmanila on 2025-08-14 08:42_

---

_Closed by @dhruvmanila on 2025-08-14 08:42_

---

_Branch deleted on 2025-08-14 08:42_

---

_@MichaReiser reviewed on 2025-08-14 08:57_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 08:57_

I'm not entirely sure about enabling all of them by default. E.g. pylance all inlays by default (which I'm also not sure if that's great). We probably want something in between where we enable the non noisy inlays by default but keep all others disabled. 

Having said that. I think defaulting those two settings to true makes sense.

I assume the new default implementation matches the defaults in the VS code settings?

---

_@dhruvmanila reviewed on 2025-08-14 08:59_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 08:59_

Hmm, good point.

I think I have it backwards. Both of them are disabled by default in Pylance.

---

_@dhruvmanila reviewed on 2025-08-14 09:36_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 09:36_

As discussed in our 1:1, we've decided to keep it enabled by default, and can change it in the future before GA based on user feedback.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 09:43_

Hmm, I'm a bit surprised by this change, and a bit unhappy with that resolution. We discussed this a fair bit as a team in the original PR adding inlay hints (https://github.com/astral-sh/ruff/pull/17214#issuecomment-2781460823, and there was also discussion on Discord as well IIRC), and I thought we came to a consensus that we should keep them disabled by default

---

_@AlexWaygood reviewed on 2025-08-14 09:43_

---

_@dhruvmanila reviewed on 2025-08-14 09:49_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 09:49_

Oh, I think I forgot about that discussion, thanks for the reminder. I'll revert this change back for now.

---

_@MichaReiser reviewed on 2025-08-14 09:58_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 09:58_

@AlexWaygood editors have a global setting to opt in/out of inlay hints. I think it's confusing if enabling inlay hints requires both: enabling them globally in the editor and for the extension. In that sense, inlays are disabled by default unless you enable them in your editor.

This behavior also matches rust-analyzers behavior.

---

_@AlexWaygood reviewed on 2025-08-14 10:03_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:03_

> This behavior also matches rust-analyzers behavior.

I find inlay hints far more useful when writing Rust than when writing Python, as I described in https://github.com/astral-sh/ruff/pull/17214#issuecomment-2781460823.

Does this behaviour match Pylance? I don't think I've ever toggled the inlay hints setting in VSCode either for the editor or for any specific extension, but I've always seen inlay hints from rust-analyzer when writing Rust, and I've never seen inlay hints from Pylance when writing Python.

---

_@MichaReiser reviewed on 2025-08-14 10:05_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:05_

> Does this behaviour match Pylance? 

It does not. Pylance has them off by default. 

I still think we should keep inlays that we find useful and aren't noisy on by default. That's what users opt-in to by enabling inlay hints at the editor level and I rather change our defaults based on user feedback. Which I think we get more of if we enable them by default and it's something we can change very easily moving forward.

E.g. I find the argumet name inlay very useful. Maybe the variable type is less so. But we should discuss them independently

---

_@MichaReiser reviewed on 2025-08-14 10:08_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:08_

I think it's a good call out that we should think about why we enable which inlays by default and we can create an issue to revisit this. In the meantime, I'd prefer to defer this discussion to later.

---

_@dhruvmanila reviewed on 2025-08-14 10:09_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:09_

That sounds good, I can create an issue and proceed with the release as usual.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:10_

I'm afraid I'm still of the opinion that this is a change that I would definitely not want to see in my editor with what I understand the current state of this feature as being; that it would take me some time to figure out how to change the settings in VSCode (I can never find my way to the right setting on the rare occasion I need to change something); that this is going to be quite surprising to users coming from Pylance; and that this is going to negatively impact most users of the ty server.

---

_@AlexWaygood reviewed on 2025-08-14 10:10_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:14_

Just for context, the variable type hints were enabled by default when it got added and it only got disabled by default in https://github.com/astral-sh/ruff/pull/19780 which I did not intend to make. That was the main motivation of this PR which is to turn it back on to return the behavior in the released ty version.

---

_@dhruvmanila reviewed on 2025-08-14 10:14_

---

_@AlexWaygood reviewed on 2025-08-14 10:22_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:97 on 2025-08-14 10:22_

> Just for context, the variable type hints were enabled by default when it got added and it only got disabled by default in #19780 which I did not intend to make. That was the main motivation of this PR which is to turn it back on to return the behavior in the released ty version.

Ahhh, this I did not realise. I'm definitely fine with restoring the status quo, in that case, and continuing the discussion in an issue elsewhere.

---
