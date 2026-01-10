```yaml
number: 13412
title: "[red-knot] dedicated error message for all-union-elements not callable"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/union-all-not-callable
created_at: 2024-09-19T22:15:05Z
updated_at: 2024-09-20T15:09:26Z
url: https://github.com/astral-sh/ruff/pull/13412
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] dedicated error message for all-union-elements not callable

---

_Pull request opened by @carljm on 2024-09-19 22:15_

This was mentioned in an earlier review, and seemed easy enough to just do it. No need to repeat all the types twice when it gives no additional information.

---

_Label `red-knot` added by @carljm on 2024-09-19 22:15_

---

_Review requested from @MichaReiser by @carljm on 2024-09-19 22:15_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-19 22:15_

---

_Comment by @github-actions[bot] on 2024-09-19 22:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-09-20 01:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-09-20 01:03_

I expect you'll call me pedantic here, but this feels like it confuses the _type_ being (not) callable with _instances of the type_ being (not) callable. What about something like this?

```suggestion
                            "No possible instance of type '{}' is callable.",
```

---

_@carljm reviewed on 2024-09-20 02:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-09-20 02:00_

After considering, I think what makes most sense is to just change this message to say `Object of type 'foo | bar' is not callable.` the same as we'd do for a non-union non-callable type. There's really no difference.

I think your objection might also apply to the existing `Union element '{}' of type '{}' is not callable.` and `Union elements {} of type '{}' are not callable.` messages, but since those aren't touched in this PR and it's not clear to me how to improve them, I'll let you propose a separate change to them if you want.

I feel like this is a mis-interpretation of what a "type" means, since "the type `Foo`" inherently means instances of Foo, so "the type `Foo` is not callable" already means "instances of Foo are not callable" -- as distinct from saying "the type `Type[Foo]` (or `type`, or `Literal[Foo]`) is not callable". So I'm not totally convinced we want to make our error messages more verbose to extra-clarify this. But if there are ways to word them that aren't super verbose and make this clearer, I don't have a problem with that.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-09-20 02:07_

To elaborate, "object of type X is not callable" feels correct to me whereas "type X is not callable" does not feel correct. It is the object that has that type — the object that is an instance of the type — that is not callable. The not-callable type itself is not a subtype of the Callable protocol, but the type itself is actually callable, of course (for the vast majority of types); that's how you create instances of the type, of course: by calling it. Or, well, perhaps you could argue that in fact you call the [constructor method of the] _class_ rather than the _type_ to construct instances of the not-callable type — but I suspect that's a subtlety that'll be lost on most of our users.

---

_@AlexWaygood reviewed on 2024-09-20 02:07_

---

_@AlexWaygood approved on 2024-09-20 02:08_

LGTM, this seems like an improvement!

---

_@AlexWaygood reviewed on 2024-09-20 02:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-09-20 02:17_

it seems like neither pyright nor mypy even considers this problem, because they simply issue a separate diagnostic for each element in the union that isn't a subtype of `Callable`, rather than trying to consolidate them into a single diagnostic like we do 😄

- https://mypy-play.net/?mypy=latest&python=3.12&gist=814f33f5253008f2698adc4913f4ac71
- https://pyright-play.net/?pythonVersion=3.13&strict=true&code=CYUwZgBGAUAeBcECWA7ALhAPhAzmgTgJTwCwAUBJRLNIeUA

---

_@MichaReiser approved on 2024-09-20 06:28_

Thanks

---

_@carljm reviewed on 2024-09-20 15:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-09-20 15:07_

I tweaked the other combined union messages so that all the messages are more consistent in form.

---

_Merged by @carljm on 2024-09-20 15:08_

---

_Closed by @carljm on 2024-09-20 15:08_

---

_Branch deleted on 2024-09-20 15:08_

---

_@AlexWaygood reviewed on 2024-09-20 15:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:829 on 2024-09-20 15:09_

Thank you!

---
