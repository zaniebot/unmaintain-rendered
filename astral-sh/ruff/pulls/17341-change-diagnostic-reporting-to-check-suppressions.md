```yaml
number: 17341
title: change diagnostic reporting to check suppressions more eagerly
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/reporter-refactor
created_at: 2025-04-10T19:26:56Z
updated_at: 2025-04-11T16:36:38Z
url: https://github.com/astral-sh/ruff/pull/17341
synced_at: 2026-01-12T15:56:01Z
```

# change diagnostic reporting to check suppressions more eagerly

---

_@BurntSushi_

This PR is meant to address some of @MichaReiser's feedback in #17318.

Micha and I spoke offline a bit about this, I ended up taking this approach:

* Rename `LintReporter` to `LintDiagnosticGuard`.
* Rename `LintReporterBuilder` to `LintDiagnosticGuardBuilder`.
* Make `LintDiagnosticGuard` impl `Deref` and `DerefMut` for `Diagnostic`. (This removes an _apparent_ layer of indirection in caller code.)
* Require a `TextRange` to construct a `LintDiagnosticGuardBuilder`.
* Make suppression checks eager instead of lazy. Now they're done when `LintDiagnosticGuardBuilder` is constructed instead of when `LintDiagnosticGuard` is dropped.
* Making a `LintDiagnosticGuard` will build a `Diagnostic` _with_ a primary annotation containing a `Span` derived from the aforementioned `TextRange`. But it _won't_ have a message attached to it.
* Expose a `LintDiagnosticGuard::set_primary_message` routine for a special access way to set a message on the aforementioned primary annotation in-place.


---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-10 19:27_

---

_Comment by @github-actions[bot] on 2025-04-10 19:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-04-10 20:41_

---

_Label `diagnostics` added by @AlexWaygood on 2025-04-10 20:41_

---

_Comment by @MichaReiser on 2025-04-11 07:20_

Thanks for putting this PR up before my PTO and giving me a chance to provide feedback. 

Reading through your changes, I now share your concern that eagerly providing the message might result in unnecessary work that your previous API allowed to skip. You work around this by adding an `IntoDiagnosticMessage` trait (we may still want that to avoid allocations, but let's ignore this for now ;)). 

What do you think of the following which is something between the two solutions with slightly different naming:

* `report_lint` only requires the lint metadata (or id?) and a text range (we take the file from `InferContext`). It returns an `Option<Builder>`. 
* The `Builder` (we can also name it `Reporter`) has a `diagnostic` method (I don't like the name, maybe you have ideas for a better name) that takes a message and returns a `DiagnosticGuard` (or `DiagnosticFlushGuard`, `DiagnosticSubmitGuard`...) 
* `DiagnosticGuard` implements `Deref`, `DerefMut` and has a `DiagnosticGuard::into_inner` method in case someone wants to get the diagnostic out (e.g. to make it a sub-diagnostic of another diagnostic). 
* `DiagnosticGuard` implements `Drop` to submit the `Diagnostic` once it's fully built. 

The reasons I prefer this approach are:

* It's unclear to me if deciding what diagnostic message to use is something that's always known upfront or requires a fair bit of computation. For example, a rule might have a generic message but we may be able to use a more specific message if we can identify that X, Y, and Z are true (which are non trivial to compute)
* The `Guard` naming mitigates most of my concerns around the `Builder`/`Reporter` split. The name makes it clear that its only responsibility is to flush the diagnostic once it goes out of scope. 


Wdyt? We can also do another quick VC if you'd find that helpful.

---

_Marked ready for review by @BurntSushi on 2025-04-11 15:18_

---

_Review requested from @carljm by @BurntSushi on 2025-04-11 15:18_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-11 15:18_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-11 15:18_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-11 15:18_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-11 15:19_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-11 15:19_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-11 15:19_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-04-11 15:19_

---

_Comment by @BurntSushi on 2025-04-11 15:27_

~~@MichaReiser and I spoke offline a bit about this, I ended up taking this approach:~~

* ~~Rename `LintReporter` to `LintDiagnosticGuard`.~~
* ~~Rename `LintReporterBuilder` to `LintDiagnosticGuardBuilder`.~~
* ~~Make `LintDiagnosticGuard` impl `Deref` and `DerefMut` for `Diagnostic`. (This removes an _apparent_ layer of indirection in caller code.)~~
* ~~Require a `TextRange` to construct a `LintDiagnosticGuardBuilder`.~~
* ~~Make suppression checks eager instead of lazy. Now they're done when `LintDiagnosticGuardBuilder` is constructed instead of when `LintDiagnosticGuard` is dropped.~~
* ~~Making a `LintDiagnosticGuard` will build a `Diagnostic` _with_ a primary annotation containing a `Span` derived from the aforementioned `TextRange`. But it _won't_ have a message attached to it.~~
* ~~Expose a `LintDiagnosticGuard::set_primary_message` routine for a special access way to set a message on the aforementioned primary annotation in-place.~~

(I copied this to my initial PR message above instead.)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1259 on 2025-04-11 15:44_

Nit: I don't think this is a huge concern but it's not very clear to readers to what the string passed to `build` corresponds to. What would you think of `with_message`. I dislike that doesn't fit well into the builder pattern and I might also be overindexing here. Definetely not important and something that we can change very easily later. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:102 on 2025-04-11 15:45_

I think the message part here is outdated. Or to what message does the comment here refer to?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:223 on 2025-04-11 15:47_

Can you give an example of one such condition? I'm not aware of a requirement that requires omitting diagnostics in drop.  I'm leaning towards removing that part of the comment for now because it was more confusing than enlightening to me.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/context.rs`:260 on 2025-04-11 15:48_

Love it :D

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:817 on 2025-04-11 15:54_

I don't know the answer to this. You probalby know better. Should this be a generic implementation over `ToString` or does it not matter because the two are roughly equivalent anyway?

---

_@MichaReiser approved on 2025-04-11 15:54_

This is great. I really like the outcome! Thanks for being open about my concerns and taking the time to iterate on the API

---

_@BurntSushi reviewed on 2025-04-11 16:13_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1259 on 2025-04-11 16:13_

Hmmm. I'm not a huge fan of `with_message` either.

What about `to_diagnostic`?

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/context.rs`:102 on 2025-04-11 16:15_

Yeah I flubbed this. Thanks. Fixed. It's referring to the message passed to `build()`.

---

_@BurntSushi reviewed on 2025-04-11 16:15_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1259 on 2025-04-11 16:16_

I went with `to_diagnostic`.

Still not a huge fan of that either, but I think it's better than `build()`.

Happy to change this to something else later.

---

_@BurntSushi reviewed on 2025-04-11 16:16_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/context.rs`:223 on 2025-04-11 16:17_

Oh I'm not aware of any either. This is just my semver API brain writing down possible eventualities. But like hypothetically, maybe we want suppressions to work on non-primary spans for example. IDK.

I tried to head off the confusion here by saying that there aren't any such cases happening now.

But maybe this is just overwrought... Fair enough. I'll remove it.

---

_@BurntSushi reviewed on 2025-04-11 16:17_

---

_@BurntSushi reviewed on 2025-04-11 16:24_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:817 on 2025-04-11 16:24_

This is a good question. `ToString` does indeed work here. And you're right, they are effectively equivalent because of the blanket impl for `ToString`. For our specific circumstance, I cannot think of a benefit or a cost to use one over the other. So I'd just assume stick to `std::fmt::Display`. It's technically a little more flexible, but I don't think it really matters.

For a library crate, you'd definitely want to use `std::fmt::Display` here though. Because that will work in even core-only environments that don't have `ToString` or a `String` or even a concept of a heap. But we don't care about that here.

---

_Merged by @BurntSushi on 2025-04-11 16:36_

---

_Closed by @BurntSushi on 2025-04-11 16:36_

---

_Branch deleted on 2025-04-11 16:36_

---
