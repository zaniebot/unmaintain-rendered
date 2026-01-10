```yaml
number: 19269
title: "[ty] Add inlay hints for call arguments"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: function-argument-inlay-hints
created_at: 2025-07-10T18:04:33Z
updated_at: 2025-08-14T08:47:31Z
url: https://github.com/astral-sh/ruff/pull/19269
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add inlay hints for call arguments

---

_Pull request opened by @MatthewMckee4 on 2025-07-10 18:04_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add function argument inlay hints.

Note that the inlay hints here have the structure `{name}=` instead of `{name}:` for rust. I think this makes more sense for python.

<img width="765" height="570" alt="image" src="https://github.com/user-attachments/assets/36a09289-ad37-4334-a028-5593d52722b8" />

As you can see I have made lots of visibility modifiers public. I don't want to do this, but I have for an initial showcase of this feature.

If anyone can advise on a better solution (maybe some function inside the `ty_python_semantic` crate) that would be greatly appreciated.

## Test Plan

Add tests to `ty_ide/src/inlay_hints.rs`. 


---

_Comment by @MatthewMckee4 on 2025-07-10 18:05_

Will mark this as ready for review for some initial feedback

---

_Marked ready for review by @MatthewMckee4 on 2025-07-10 18:05_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-10 18:05_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-10 18:05_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-10 18:05_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-10 18:05_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-07-10 18:05_

---

_Label `server` added by @AlexWaygood on 2025-07-10 18:06_

---

_Label `ty` added by @AlexWaygood on 2025-07-10 18:06_

---

_Comment by @github-actions[bot] on 2025-07-10 18:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-10 19:39_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:155 on 2025-07-11 07:22_

Nit: Do we need to clone here?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:156 on 2025-07-11 07:23_

I think we need to match the signature here to resolve the best matching overload or it will show incorrect results for overloads

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1126 on 2025-07-11 07:24_

Huh, why do we need to call `into_callable` twice. This looks fairly odd

---

_@MichaReiser reviewed on 2025-07-11 07:24_

---

_@MatthewMckee4 reviewed on 2025-07-11 07:25_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:155 on 2025-07-11 07:25_

No, I forgot to remove that thanks

---

_@MatthewMckee4 reviewed on 2025-07-11 07:26_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1126 on 2025-07-11 07:26_

ClassLiteral.into_callable can return bound methods, I agree it looks weird, maybe I should change the class literal function instead to convert them

---

_@MatthewMckee4 reviewed on 2025-07-11 07:28_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:156 on 2025-07-11 07:28_

Yeah that makes sense, I just took the first one for initial demonstration. I'll look into to how to do that

---

_@MichaReiser reviewed on 2025-07-11 08:26_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:156 on 2025-07-11 08:26_

It could make sense to take a look at the `signature_help` implementation which does something very similar.

---

_@MatthewMckee4 reviewed on 2025-07-11 08:29_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:156 on 2025-07-11 08:29_

Okay thank you

---

_@MatthewMckee4 reviewed on 2025-07-11 18:54_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:156 on 2025-07-11 18:54_

This works well, but it also provides inlay hints for positional only, variadic and keyword_variadic, which i don't think we want

---

_@MatthewMckee4 reviewed on 2025-07-11 19:25_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1126 on 2025-07-11 19:25_

I have added more into_callable calls in `ClassLiteral.into_callable`

---

_@MatthewMckee4 reviewed on 2025-07-11 19:45_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:156 on 2025-07-11 19:45_

Fixed that now

---

_Comment by @MatthewMckee4 on 2025-07-11 20:05_

This doesn't seem to work well with overloads. Seems to be because of one of the internal functions from bindings maybe

---

_Converted to draft by @MichaReiser on 2025-07-14 07:51_

---

_Comment by @MatthewMckee4 on 2025-07-24 23:24_

@MichaReiser what are you're thoughts on this PR, what more would you like to see?

Also, is this something we really want?

---

_Comment by @github-actions[bot] on 2025-08-05 22:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @MatthewMckee4 on 2025-08-05 22:24_

I'm aware what I've changed in into callable is out of scope, I'll try to move that to another PR

---

_Marked ready for review by @MatthewMckee4 on 2025-08-06 22:08_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-06 22:08_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:62 on 2025-08-07 12:05_

Is this not given by the visiting order in `SourceOrderVisitor`?

I think I see. It's because of how we walk functions where we add the function argument inlays before we visit the arguments. Would it be possible to change the visiting order so that the sort can be avoided? It's not hugely important but would be nice. 

I also think we can use `sort_unstable_by`. It's fine if two inlay for the same position (which shouldn't happen anyway) get reordered.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:167 on 2025-08-07 12:07_

Can we call walk_expr instead of walking the tree manually or are we intentionally skipping some fields?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:851 on 2025-08-07 12:09_

Can this be `pub(crate)` or even private?

---

_@MichaReiser approved on 2025-08-07 12:12_

> Note that the inlay hints here have the structure {name}= instead of {name}: for rust. I think this makes more sense for python.

Have you looked into what other Python language servers (e.g. pylance) do here?

Nice. This looks great. We can now also gate this behind an option similar to what @dhruvmanila did in https://github.com/astral-sh/ruff/pull/19780

---

_@MatthewMckee4 reviewed on 2025-08-07 13:24_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:167 on 2025-08-07 13:24_

I can properly walk the tree as normal, I was just having issues with ordering, but will review this section again

---

_Comment by @dhruvmanila on 2025-08-07 14:14_

> We can now also gate this behind an option similar to what @dhruvmanila did in #19780

For reference, this is what Python extension provides:

```ts
  // Enable/disable inlay hints for call argument names:
  // ```python
  // datetime('year='2019, 'month='10, 'day='27)
  // ```
  // 
  //  - off: Disable inlay hints for call argument names.
  //  - partial: Enable inlay hints for positional-or-keyword arguments while ignoring positional-only and keyword-only.
  //  - all: Enable inlay hints for positional-or-keyword and positional-only arguments while ignoring keyword-only.
  "python.analysis.inlayHints.callArgumentNames": "off",
```

For now, we can just start with a boolean `true` / `false` but the "partial" does seem interesting although not sure about usage metrics.

---

_Comment by @MatthewMckee4 on 2025-08-07 17:47_

Cool, thanks. To be honest I didn't know the python extension provided this

---

_Comment by @MatthewMckee4 on 2025-08-07 17:53_

Should we start with Boolean for all inlay hints or for both call arguments and assignments

---

_@MatthewMckee4 reviewed on 2025-08-07 17:58_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:62 on 2025-08-07 17:58_

Yeah I'll play around with the order, I should be able to remove the sort

---

_@MatthewMckee4 reviewed on 2025-08-07 19:40_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:851 on 2025-08-07 19:40_

this is used in ty_ide

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-08-07 19:42_

---

_Comment by @MichaReiser on 2025-08-08 07:49_

> Should we start with Boolean for all inlay hints or for both call arguments and assignments

I suggest following pylance's approach with a separate boolean option for each (we can change it to a non boolean option in the future)

> Have you looked into what other Python language servers (e.g. pylance) do here?

Were you able to find out how pylance handle call argument names (e.g. how they're formatted?)

---

_Comment by @MatthewMckee4 on 2025-08-08 08:17_

They do it like it is done in this PR.

They also have some guard that prevents argument names prefixed with "_" being displayed, that seems beneficial to add here

---

_Comment by @MichaReiser on 2025-08-11 14:24_

Just checking in (no pressure). Is this waiting on any input from my end?

---

_Comment by @MatthewMckee4 on 2025-08-11 20:44_

Sorry, no not really, I just haven't got to doing what I said was beneficial.

I'll get that done, then I think I'm happy with this.

---

_Comment by @MichaReiser on 2025-08-12 06:39_

Okay, no worries. I just wanted to make sure it isn't blocked on me. 

---

_Comment by @MatthewMckee4 on 2025-08-12 13:46_

Okay, that's all the changes I think i need to make

---

_Merged by @MichaReiser on 2025-08-12 13:56_

---

_Closed by @MichaReiser on 2025-08-12 13:56_

---

_Branch deleted on 2025-08-12 13:57_

---

_Comment by @MichaReiser on 2025-08-12 14:21_

Again, this is great. Thank you. I'm not yet sure how many inlay hints we want to enable in the playground but that's something we can easily tune

---

_Renamed from "[ty] Function argument inlay hints" to "[ty] Add inlay hints for call arguments" by @dhruvmanila on 2025-08-14 08:47_

---
