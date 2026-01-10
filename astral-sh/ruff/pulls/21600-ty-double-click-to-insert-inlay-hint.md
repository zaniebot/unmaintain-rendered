```yaml
number: 21600
title: "[ty] Double click to insert inlay hint"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: bake-inlay-hints
created_at: 2025-11-24T00:14:32Z
updated_at: 2025-12-27T00:21:12Z
url: https://github.com/astral-sh/ruff/pull/21600
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Double click to insert inlay hint

---

_Pull request opened by @MatthewMckee4 on 2025-11-24 00:14_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/317#issuecomment-3567398107.

I can't get the auto import working great.

I haven't added many places where we specify that the type display is invalid syntax.

## Test Plan

Nothing yet


---

_Comment by @astral-sh-bot[bot] on 2025-11-24 00:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 00:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:110 on 2025-11-24 00:20_

This causes issues for some definition kinds, like `Unknown`.

---

_@MatthewMckee4 reviewed on 2025-11-24 00:20_

---

_Renamed from "Double click to insert inlay hint" to "[ty] Double click to insert inlay hint" by @AlexWaygood on 2025-11-24 00:22_

---

_Label `server` added by @AlexWaygood on 2025-11-24 00:22_

---

_Label `ty` added by @AlexWaygood on 2025-11-24 00:22_

---

_Comment by @Gankra on 2025-11-24 14:43_

The parts that aren't the auto-import stuff look reasonable to me (just not as familiar with auto-import stuff).

Might make sense to have the auto-import stuff be a follow-up?

---

_Comment by @MatthewMckee4 on 2025-11-24 15:25_

Yes, I agree. Perhaps @BurntSushi would be able to advise or help with the auto import?

I can remove the auto import for now. 

Perhaps we can put this behind a experimental flag for now also?

I will also try to add more invalid syntax places.

I also think it would be useful if we had alternative display for types that emit invalid synatx, like I'd still like to add a function signature with `Callable` notation.
This also seems like something we can just iterate on. 

---

_Comment by @Gankra on 2025-11-24 15:28_

> Perhaps we can put this behind a experimental flag for now also?

I'm on the fence about it because you can just... not double-click (and undo if you do). I think we can land the minimal implementation and then disable/flag it if it's a problem for some reason.

>  This also seems like something we can just iterate on.

Yeah agreed.

---

_Comment by @Gankra on 2025-11-24 15:29_

I don't think it's strictly a blocker to not catch all the invalid syntax. It's something we'll probably burn down really fast in followups and the only stakes is a user getting mad that a feature they manually invoked did something useless.

---

_Comment by @Gankra on 2025-11-24 15:30_

Actually ok I think it's slightly higher stakes -- "user gets very confused" is probably more likely. But still, followups.

---

_Comment by @MatthewMckee4 on 2025-11-24 15:31_

> I'm on the fence about it because you can just... not double-click

true, but at least for now if you double click on some type with a `todo` type, its looks horribly and not an amazing experience.

Though, we may not get the user feedback about missing invalid syntax types if we hid it.

Happy to keep it open now, and let the issues trickle in.

---

_@BurntSushi reviewed on 2025-11-24 15:31_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/inlay_hints.rs`:123 on 2025-11-24 15:31_

What you have here looks reasonable to me. What's going wrong?

---

_@MatthewMckee4 reviewed on 2025-11-24 15:34_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:123 on 2025-11-24 15:34_

I will review more tonight. But off the top of my head.

```py
T[: typing.TypeVar] = TypeVar("T")
```
the definition name is `TypeVar` so we `from typing import TypeVar` for example, but this doesnt help us. as we now have the annotation `: typing.TypeVar`.

Obviously not a great example because we should not type hint `TypeVar` (also a good place to not allow insert option`.

But for other types this would cause problems.

So its mostly an issue with the names. 

---

_Comment by @Gankra on 2025-11-24 15:40_

A rough heuristic for invalid syntax that will let you find most things: i believe `(`, `)`, `<`, `>`, `'` are all invalid in type syntax (idk if `'` is stricly invalid but anywhere we explicitly emit it we're doing type crimes).

---

_Comment by @MatthewMckee4 on 2025-11-24 15:44_

Nice okay, do you think its worth it for us to scan over the label after getting the TypeDetails to check for invalid characters? Or should i just search for them in the display.rs file and manually set them to all be invalid

---

_Comment by @Gankra on 2025-11-24 15:47_

It's really tempting to just do the `contains` but I think it's probably best to mark up our code in display.rs yeah.

---

_@BurntSushi reviewed on 2025-11-24 15:55_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/inlay_hints.rs`:123 on 2025-11-24 15:55_

Oh I see. You might need to use `ImportAction::symbol_text`, since the import might need to be fully qualified for correctness in some cases.

With that said, I think the specific issue you're hitting is that you're always requesting a `from import` statement via `ImportRequest::import_from`. If you want to respect an existing qualified import, then you'll want `ImportRequest::import`. So you'll probably need to detect which one you have and then use the right request. (Note though that it is only a _request_. So you still need to use `ImportAction::symbol_text`.)

I built the importer API with completions in mind. It might make sense to change `ImportAction::symbol_text` to return an `Edit` instead of a `&str` for your use case.

---

_@MatthewMckee4 reviewed on 2025-11-24 16:23_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:123 on 2025-11-24 16:23_

Okay, thanks. I'll leave this out for this PR i think, and have a go at it separately.

One thing i am thinking is that I don't know how many different formats of types we have like this. If it were just `TypeVar` and `typing.TypeVar` I could easily distinguish between them, but I don't know other ways this could show.

---

_Comment by @MatthewMckee4 on 2025-11-24 16:35_

Is it ridiculous to try to make some property tests for this?

testing that after the edits have been accepted, that we have valid python syntax? And possibly no diagnostics on the file?

---

_Comment by @Gankra on 2025-11-24 16:37_

More tests is more better, I would have been lazy and just snapshot-tested like the rest of the IDE stuff 😅 

---

_Comment by @MatthewMckee4 on 2025-11-24 16:38_

I will try it then, see how it goes.

---

_Comment by @Gankra on 2025-11-24 16:44_

I will say we have a release tomorrow and I'd love to get some version of this PR in for that release (not a huge deal if not but I think this will really solidify inlay hints as Useful).

---

_@MatthewMckee4 reviewed on 2025-11-24 17:47_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:986 on 2025-11-24 17:47_

We should not edit here.

---

_Marked ready for review by @MatthewMckee4 on 2025-11-24 18:22_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-24 18:22_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-24 18:22_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-24 18:22_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-24 18:22_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-24 18:22_

---

_@AlexWaygood reviewed on 2025-11-24 18:29_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:123 on 2025-11-24 18:29_

FWIW, `T: TypeVar = TypeVar("T")` is an invalid TypeVar declaration that [ty will complain about](https://play.ty.dev/a43cc2d3-739f-42a2-a290-d6b9a57b6e6a), so we maybe shouldn't be offering a code action to add that to the user's code 😅

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:451 on 2025-11-24 19:01_

Oh great call on this -- I might also call this `is_valid_syntax`, since that's ultimately the crux?

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:5805 on 2025-11-24 19:04_

(can be a followup) i have a sneaking suspicion this is a fake type

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/definition.rs`:67 on 2025-11-24 19:08_

Is this code from the auto-import stuff?

---

_@Gankra approved on 2025-11-24 19:08_

Rad! Just a few nits.

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/definition.rs`:67 on 2025-11-24 19:13_

yeah, will remove, thanks

---

_@MatthewMckee4 reviewed on 2025-11-24 19:13_

---

_@MatthewMckee4 reviewed on 2025-11-24 19:13_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:5805 on 2025-11-24 19:13_

yeah this didnt look great

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:451 on 2025-11-24 19:18_

I guess its "if we add an annotation is it valid syntax".

---

_@MatthewMckee4 reviewed on 2025-11-24 19:18_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:451 on 2025-11-24 19:22_

But I'm not sure how to name that, "is_valid_syntax" doesn't seem exactly correct.


---

_@MatthewMckee4 reviewed on 2025-11-24 19:22_

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:451 on 2025-11-24 19:24_

annotations_are_valid_syntax?

---

_@Gankra reviewed on 2025-11-24 19:24_

---

_Merged by @Gankra on 2025-11-24 19:48_

---

_Closed by @Gankra on 2025-11-24 19:48_

---

_Comment by @Gankra on 2025-11-24 20:01_

Hell yes hell yeah let's kick this things tires

---

_Branch deleted on 2025-12-27 00:21_

---
