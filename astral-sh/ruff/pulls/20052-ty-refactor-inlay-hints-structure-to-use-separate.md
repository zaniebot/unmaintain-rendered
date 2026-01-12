```yaml
number: 20052
title: "[ty] Refactor inlay hints structure to use separate parts"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: refactor-inlay-hints
created_at: 2025-08-22T21:22:34Z
updated_at: 2025-11-06T11:48:22Z
url: https://github.com/astral-sh/ruff/pull/20052
synced_at: 2026-01-12T15:56:53Z
```

# [ty] Refactor inlay hints structure to use separate parts

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Our internal inlay hints structure (`ty_ide::InlayHint`) now more closely resembles `lsp_types::InlayHint`.

This mainly allows us to convert to `lsp_types::InlayHint` with less hassle, but it also allows us to manage the different parts of the inlay hint better, which in the future will allow us to implement features like goto on the type part of the type inlay hint.

It also really isn't important to store a specific `Type` instance in the `InlayHintContent`. So we remove this and use `InlayHintLabel` instead which just shows the representation of the type (along with other information). 

We see a similar structure used in rust-analyzer too. 


---

_Marked ready for review by @MatthewMckee4 on 2025-08-22 21:24_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-22 21:24_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-22 21:24_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-22 21:24_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-22 21:24_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-22 21:24_

---

_Comment by @github-actions[bot] on 2025-08-22 21:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-22 21:26_

---

_Renamed from "Refactor inlay hints" to "[ty] Refactor ty_ide inlay hints" by @MatthewMckee4 on 2025-08-22 21:26_

---

_@MatthewMckee4 reviewed on 2025-08-22 21:30_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/tests/e2e/inlay_hints.rs`:45 on 2025-08-22 21:30_

These change because we now use `InlayHintLabel::LabelParts` instead of `InlayHintLabel::String`

---

_Comment by @github-actions[bot] on 2025-08-22 21:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-08-22 21:57_

---

_Label `ty` added by @AlexWaygood on 2025-08-22 21:57_

---

_Review request for @carljm removed by @carljm on 2025-08-22 23:12_

---

_Review requested from @dhruvmanila by @carljm on 2025-08-22 23:12_

---

_Converted to draft by @MatthewMckee4 on 2025-08-22 23:17_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:24 on 2025-08-22 23:35_

This is roughly how rust analyzer does it and we need to separate these for the goto targets

---

_@MatthewMckee4 reviewed on 2025-08-22 23:35_

---

_@MatthewMckee4 reviewed on 2025-08-22 23:36_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:58 on 2025-08-22 23:36_

I think i should change this to 2. @MichaReiser would you agree?

---

_@MatthewMckee4 reviewed on 2025-08-22 23:37_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:58 on 2025-08-22 23:37_

ive done it, i think it makes sense for us.

---

_@MatthewMckee4 reviewed on 2025-08-22 23:42_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:58 on 2025-08-22 23:42_

ah okay in this situation we only use 1 of the `InlayHintLabelPart`, but when we add the goto, we will use 2.

---

_Marked ready for review by @MatthewMckee4 on 2025-08-22 23:43_

---

_Comment by @MichaReiser on 2025-08-23 12:33_

Could you extend the summary? I think I've an idea why we may want this but I'd rather be sure (and documenting this will also help a future reader who comes across this PR and wonders why it works the way it does)

---

_@MichaReiser reviewed on 2025-08-23 12:37_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:58 on 2025-08-23 12:37_

I think it makes sense today because all our usages have exactly two parts. But it's hard to tell, depending on how you plan to use this going forward. 

A too-large number has the downside that `InlayHintLabel` becomes larger overall, wasting memory when we only need to store 1 or more than two elements. This is especially problematic if we create many `InlayHintLabel`s. I don't think this is a big concern here.

I'm not even sure if it's worth using a `SmallVec` here. We'll need to serialize all that data later on, we might as well just store a Vec


---

_@MatthewMckee4 reviewed on 2025-08-24 18:53_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:58 on 2025-08-24 18:53_

Currently because we don't use the `target` variable in `InlayHintLabel`. This means we just have one `part` so we will only use one of the elements in the small vec.

I'll change to vec for now anyway.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:20 on 2025-08-25 08:00_

I think it'd be useful to add basic documentation for all the `pub` items in this module.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:61 on 2025-08-25 08:03_

I think `simple` sounds confusing, what does simple mean in the context of `InlayHintLabel`? Based on my other suggestion to introduce `with_target` for `InlayHintLabelPart`, we could rename this to `single(part: InlayHintLabelPart)` and let the caller construct the `InlayHintLabelPart` or just a `new(parts: Vec<InlayHintLabelPart>`) as `InlayHintLabelPart` is a public type.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:58 on 2025-08-25 08:04_

Are you thinking of expanding `InlayHintLabel` with additional fields in the future?

Regardless, I think the `parts` field should be private and we should expose this information via a method that returns `&[InlayHintLabelPart]` instead.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:73 on 2025-08-25 08:05_

At first, I thought that the catch-all branch is for the empty vector (`[]`) but I don't think that's true, the branch that isn't covered here is an `InlayHintLabelPart` where `target` is `Some`. Can you expand on why does the string needs to be added only when navigation target is not present? Based on the method name, I would expect this to add the string regardless of whether target is present or not.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:86 on 2025-08-25 08:08_

Same question as above, why should this append string only when navigation target is not present?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:129 on 2025-08-25 08:11_

I would recommend to keep the field private and expose methods to provide the information.

Additionally, I think we should have a `From<&str>` / `From<String>` / `new(text: String)` implementation for `InlayHintLabelPart` and use builder pattern for the target like `with_target` because it's optional.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:107 on 2025-08-25 08:17_

Let's keep the `inlay_hint` field private, I don't think we need to expose it publicly.

---

_@dhruvmanila reviewed on 2025-08-25 08:18_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-08-25 08:20_

---

_@MatthewMckee4 reviewed on 2025-08-25 10:03_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:20 on 2025-08-25 10:03_

I'll try to make more things private 

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:129 on 2025-08-25 10:21_

going to leave the `with_target` method for now as we don't use it yet.

---

_@MatthewMckee4 reviewed on 2025-08-25 10:21_

---

_@MatthewMckee4 reviewed on 2025-08-25 10:22_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:73 on 2025-08-25 10:22_

Ive just removed these methods, i think its better for us to just be more explicit with what the specific parts are anyway.

---

_@MatthewMckee4 reviewed on 2025-08-25 10:22_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:61 on 2025-08-25 10:22_

I have removed this too.

---

_Comment by @dhruvmanila on 2025-08-25 13:21_

Feel free to re-request for review once it's ready.

---

_Review requested from @dhruvmanila by @MatthewMckee4 on 2025-08-25 15:26_

---

_Comment by @MatthewMckee4 on 2025-08-25 15:26_

could you re run that cargo test, looks like a flake

---

_Label `internal` added by @dhruvmanila on 2025-08-26 04:47_

---

_Renamed from "[ty] Refactor ty_ide inlay hints" to "[ty] Refactor inlay hints structure to use separate parts" by @dhruvmanila on 2025-08-26 04:47_

---

_@dhruvmanila reviewed on 2025-08-26 04:49_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:73 on 2025-08-26 04:49_

Is this because that would allow us to have the navigation target that is relevant for that specific part of the label? For example, in `: Type`, it's just the "Type" part that's possible to do go to definition.

---

_@dhruvmanila approved on 2025-08-26 04:50_

Thanks!

---

_Comment by @dhruvmanila on 2025-08-26 04:50_

> could you re run that cargo test, looks like a flake

I'm assuming that this was fixed?

---

_Merged by @dhruvmanila on 2025-08-26 04:51_

---

_Closed by @dhruvmanila on 2025-08-26 04:51_

---

_Branch deleted on 2025-11-06 11:48_

---
