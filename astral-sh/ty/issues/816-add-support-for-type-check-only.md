```yaml
number: 816
title: "Add support for `@type_check_only`"
type: issue
state: closed
author: AlexWaygood
labels: []
assignees: []
created_at: 2025-07-11T13:25:20Z
updated_at: 2025-11-14T17:09:03Z
url: https://github.com/astral-sh/ty/issues/816
synced_at: 2026-01-12T15:54:24Z
```

# Add support for `@type_check_only`

---

_@AlexWaygood_

We should add support for [the `type_check_only` decorator](https://docs.python.org/3/library/typing.html#typing.type_check_only). Any class or function with this decorator should never be included in autocomplete suggestions. We should possibly also emit a warning if a class or function decorated with `@type_check_only` is imported into a non-stub file.

We'll want to add a `TypeCheckOnly` variant to the `KnownFunction` enum at https://github.com/astral-sh/ruff/blob/7533a0bfdbdaa7966fac056fb48d267afdb97d10/crates/ty_python_semantic/src/types/function.rs#L885. Then we'll want to filter out functions and classes with that decorator from the results returned by the `all_members` function: https://github.com/astral-sh/ruff/blob/7533a0bfdbdaa7966fac056fb48d267afdb97d10/crates/ty_python_semantic/src/types/ide_support.rs#L244.

We should probably apply these semantics to something decorated with `@type_check_only` regardless of whether the class/function was originally defined in a stub file or a `.py` file. You might plausibly define a class/function in an `if TYPE_CHECKING` block in a `.py` file and decorate it with `@type_check_only` to indicate that it won't be available at runtime.

---

_Label `help wanted` added by @AlexWaygood on 2025-07-11 13:25_

---

_Comment by @lipefree on 2025-08-05 09:33_

> We'll want to add a `NoTypeCheck`

Do you mean a `TypeCheckOnly` variant ? I will be happy to work on that !

---

_Comment by @AlexWaygood on 2025-08-05 09:58_

> Do you mean a `TypeCheckOnly` variant ? I will be happy to work on that !

oops, yes -- edited my post to correct it! And, great, thank you!

---

_Assigned to @lipefree by @AlexWaygood on 2025-08-05 09:58_

---

_Unassigned @lipefree by @AlexWaygood on 2025-10-07 21:11_

---

_Label `completions` added by @AlexWaygood on 2025-10-07 21:11_

---

_Comment by @decorator-factory on 2025-10-15 19:03_

Hey, I'd like to work on this

---

_Assigned to @decorator-factory by @AlexWaygood on 2025-10-15 19:08_

---

_Comment by @MichaReiser on 2025-10-15 19:26_

I feel like we should still return those classes, but with a very low ranking. You might still want to import that type, and just hiding it seems weird, as it clearly exists. 

---

_Comment by @MichaReiser on 2025-11-14 08:33_

I understand that this was completed as part of [#20910](https://github.com/astral-sh/ruff/pull/20910)

---

_Closed by @MichaReiser on 2025-11-14 08:33_

---

_Comment by @AlexWaygood on 2025-11-14 08:47_

> I understand that this was completed as part of [#20910](https://github.com/astral-sh/ruff/pull/20910)

That only completed the autocomplete side of things. We should also emit a diagnostic if a user tries to import one of these symbols in a .py file outside of a `TYPE_CHECKING` block.

There's also some open questions about how we might extend https://github.com/astral-sh/ruff/pull/20910 so that type-check-only symbols from other files are ranked lower in auto-import completion suggestions (https://github.com/astral-sh/ruff/pull/20910)

---

_Reopened by @AlexWaygood on 2025-11-14 08:47_

---

_Label `help wanted` removed by @AlexWaygood on 2025-11-14 08:47_

---

_Unassigned @decorator-factory by @AlexWaygood on 2025-11-14 08:48_

---

_Label `completions` removed by @MichaReiser on 2025-11-14 09:29_

---

_Label `diagnostics` added by @MichaReiser on 2025-11-14 09:29_

---

_Comment by @MichaReiser on 2025-11-14 09:29_

CC: @carljm whether this should make it into stable

---

_Comment by @AlexWaygood on 2025-11-14 09:34_

> CC: [@carljm](https://github.com/carljm) whether this should make it into stable

It doesn't need to be, I don't think. I think it would benefit users since typeshed uses this decorator extensively, but support for the feature is sketchy among other type checkers.

---

_Label `diagnostics` removed by @AlexWaygood on 2025-11-14 09:34_

---

_Comment by @AlexWaygood on 2025-11-14 17:09_

Okay, I've opened three more focused followup issues:
- https://github.com/astral-sh/ty/issues/1559
- https://github.com/astral-sh/ty/issues/1557
- https://github.com/astral-sh/ty/issues/1558

---

_Closed by @AlexWaygood on 2025-11-14 17:09_

---
