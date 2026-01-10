```yaml
number: 21417
title: "[ty] Resolve overloads for hovers"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/overloads
created_at: 2025-11-13T01:21:57Z
updated_at: 2025-11-20T17:45:05Z
url: https://github.com/astral-sh/ruff/pull/21417
synced_at: 2026-01-10T16:48:01Z
```

# [ty] Resolve overloads for hovers

---

_Pull request opened by @Gankra on 2025-11-13 01:21_

This is a very conservative minimal implementation of applying overloads to resolve a callable-type-being-called down to a single function signature on hover. If we ever encounter a situation where the answer doesn't simplify down to a single function call, we bail out to preserve prettier printing of non-raw-Signatures.

The resulting Signatures are still a bit bare, I'm going to try to improve that in a followup to improve our Signature printing in general.

Fixes https://github.com/astral-sh/ty/issues/73

---

_Label `server` added by @Gankra on 2025-11-13 01:21_

---

_Label `ty` added by @Gankra on 2025-11-13 01:21_

---

_Comment by @Gankra on 2025-11-13 01:24_

~~Most of these snapshot changes are bad and the consequence of this logic firing too eagerly. I need to cleanup that part, but I wanted to get the Types and Semantics part up sooner than later for correction.~~

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 01:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 01:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @Gankra on 2025-11-13 20:33_

---

_Review requested from @carljm by @Gankra on 2025-11-13 20:33_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-13 20:33_

---

_Review requested from @sharkdp by @Gankra on 2025-11-13 20:33_

---

_Review requested from @dcreager by @Gankra on 2025-11-13 20:33_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-13 20:33_

---

_@Gankra reviewed on 2025-11-13 20:40_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1023 on 2025-11-13 20:40_

I spent an *unconscionable* amount of time trying to have this return a Type or something more useful than a String but the types on `Binding` are haunted by the overloads when you try to render them (meaning printing them still returns the full overload set!). 

Stringifying the raw Signature is the only thing that seems to be safe.

---

_@Gankra reviewed on 2025-11-13 20:45_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:1023 on 2025-11-13 20:45_

(An early version returned a full CallSignatureDetails but this usecase has no use of that so I dropped it as a waste of effort. Also this version that just returns a String is easier to iterate on (I kept trying to Not have a String and had to refactor like 4 calls and 2 types over and over as I experimented between `Type` vs `Vec<Type>` vs `CallSignatureDetails` vs...))

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:100 on 2025-11-13 20:46_

This is debris from versions where this was being asked to store `CallSignatureDetails`, which is actually invariant over `'db`. Leaving it like this just simplifies any potential future work.

---

_@Gankra reviewed on 2025-11-13 20:46_

---

_Review request for @carljm removed by @carljm on 2025-11-13 23:31_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-14 18:39_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-14 18:39_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-14 18:39_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-14 18:39_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/hover.rs`:33 on 2025-11-14 18:42_

Is there a reason we only apply overload simplification for hover mode? For example, TypeScript jumps to the matching overload if you use "go to definition," which matches my expectation. https://www.typescriptlang.org/play/?#code/GYVwdgxgLglg9mABFApgZygCgIYC5FggC2ARigE4CU+hpFA3AFCiSwLLpZ4HFnkA0iEvgzkYYAObVEo8RKYto8JKgw4avCoJIB+EVDGTp2MAE9EAb0aMAvtcaqsARgAMggESP3le0A

I'm surprised that Pylance doesn't seem to do that

---

_@MichaReiser reviewed on 2025-11-14 21:28_

I'll do a full review on Monday but I've a question why we limit overload matching to hover only

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:995 on 2025-11-17 08:10_

Is my understanding correct that this adds another "todo" call-site to https://github.com/astral-sh/ty/issues/1086?

---

_@MichaReiser reviewed on 2025-11-17 08:13_

---

_@dhruvmanila reviewed on 2025-11-17 08:30_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/ide_support.rs`:1024 on 2025-11-17 08:30_

I think I remember now facing a similar problem regarding using the `signature_type` on `Binding` struct that we saw during our call last week. For context, the `signature_type` would represent the `FunctionLiteral` which means the display would contain all the overloads and this is the main reason we need to use the display of `Signature` to display the _matching_ overload which leads to requiring https://github.com/astral-sh/ruff/pull/21438.

I faced this issue while implementing https://github.com/astral-sh/ruff/pull/18452 and the way I resolved it is by capturing the required information (`FunctionType`, `BoundMethodType`, etc. and the matching overload index) in [`MatchingOverloadLiteral`](https://github.com/astral-sh/ruff/blob/dcc451d4d2fcf76b4c930f6aaa2f66b4830f7a04/crates/ty_python_semantic/src/types/call/bind.rs#L4130-L4141) and implementing a [`.get`](https://github.com/astral-sh/ruff/blob/dcc451d4d2fcf76b4c930f6aaa2f66b4830f7a04/crates/ty_python_semantic/src/types/call/bind.rs#L4144-L4155) method to return the matching `OverloadLiteral`.

I think we could possibly use a similar approach to get the actual `OverloadLiteral` that needs to be displayed as it does [implement the `Display` trait](https://github.com/astral-sh/ruff/blob/dcc451d4d2fcf76b4c930f6aaa2f66b4830f7a04/crates/ty_python_semantic/src/types/display.rs#L757-L774). You might find it useful to go through this part of the code: https://github.com/astral-sh/ruff/blob/dcc451d4d2fcf76b4c930f6aaa2f66b4830f7a04/crates/ty_python_semantic/src/types/call/bind.rs#L2125-L2162

Another approach could be to expand the `DisplaySettings` to include the `matching_overload_index: MatchingOverloadIndex` which can then be used directly to filter the overloads at the display level. The `None` variant would display all the overloads.

---

_@carljm reviewed on 2025-11-17 17:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:995 on 2025-11-17 17:56_

I'm not sure if we'll be able to do https://github.com/astral-sh/ty/issues/1086; commented on that issue. 

---

_@Gankra reviewed on 2025-11-17 21:44_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:33 on 2025-11-17 21:44_

Hmm strictly speaking I don't think goto-definition should jump to an overload unless we can't find the actual implementation (in which case it's actually goto-declaration). goto-declaration should ideally jump to the overload though.

---

_Comment by @MichaReiser on 2025-11-20 09:10_

@Gankra what's the status of this PR?

---

_Comment by @Gankra on 2025-11-20 17:01_

I think this PR could be landed as-is as an incremental strict improvement. I've been locally experimenting with alternative approaches that would e.g. empower goto-declaration as well, but I keep running into issues around the fact that randomly places in the code (like type printing) don't want to talk about A Specific Overload (connected to the fact that definitions-hanging-off-types is a fundamental hack).

---

_Comment by @MichaReiser on 2025-11-20 17:31_

I'm fine with handling go-to separately. But we might want to address @dhruvmanila's feedback before landing?

---

_Comment by @Gankra on 2025-11-20 17:38_

Yeah dhruv's suggestions are what I was experimenting with locally. Hover doesn't really benefit from it (it really just needs a String, and I'm computing that String fine), but goto-decl would benefit.

---

_@MichaReiser approved on 2025-11-20 17:43_

Alright. I'm fine with this, given that you're already working on the follow ups. If you don't end up implementing all of them, would you mind opening follow up issues in that case.

---

_Comment by @Gankra on 2025-11-20 17:44_

Or well, I suppose dhruv's suggestions would ideally obsolete the more hacky stuff in #21438 

But yeah it's all kinda the same work.

---

_Merged by @Gankra on 2025-11-20 17:45_

---

_Closed by @Gankra on 2025-11-20 17:45_

---

_Branch deleted on 2025-11-20 17:45_

---
