```yaml
number: 17238
title: "[red-knot] improve function and class literal type display"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-04-06T18:36:29Z
updated_at: 2025-05-07T00:11:26Z
url: https://github.com/astral-sh/ruff/issues/17238
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] improve function and class literal type display

---

_Issue opened by @carljm on 2025-04-06 18:36_

The `Literal[..]` display doesn't match the spec or other type checkers and is confusing.

The display for function types should include the signature.

---

_Label `red-knot` added by @carljm on 2025-04-06 18:36_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-06 18:36_

---

_Renamed from "[red-knot] improve functiona and class literal type display" to "[red-knot] improve function and class literal type display" by @carljm on 2025-04-06 19:07_

---

_Comment by @mishamsk on 2025-04-06 20:10_

I'll take it!

---

_Assigned to @mishamsk by @carljm on 2025-04-06 20:13_

---

_Comment by @MichaReiser on 2025-04-06 21:16_

In which places is it confusing. In our mdtests or on hover/inlay hints? 

I'm asking because on hover needs custom rendering anyway to render pretty markdown and we could also use a different render for inlay hints

---

_Comment by @carljm on 2025-04-06 21:43_

I think we should avoid significant differences between our rendering of types in any of those places; it will just cause more confusion if we aren't even consistent about how a type is spelled. There may of course be some presentation-related details that differ, but the basic text we use to represent a given type should always be the same.

---

_Comment by @MichaReiser on 2025-04-07 06:39_

Sounds painful to update all our mdtests.

I do expect some difference between reveal_type and on-hover where on-hover will only show the signature (but not the type) for classes, functions, methods, and maybe more. E.g. pyright renders a function as `def foo(a: int) -> str` 

---

_Comment by @AlexWaygood on 2025-04-07 10:40_

I agree that the type we display when users call `reveal_type` on a function-literal or class-literal is currently quite confusing and that it should be high priority for us to improve it.

---

_Comment by @mishamsk on 2025-04-07 14:11_

I was assuming we are just changing from `Literal[some_type.some_bound_symbol]` to `(args...) -> return_ty` for `reveal_type` and by extension LSP.

> Sounds painful to update all our mdtests.

Yes. But I've already done such things, I am fine doing this as long as we quickly merge a PR, so I do not have to constantly update test files as main is moving ahead.

Are we aligned on the scope here? @carljm @MichaReiser @AlexWaygood 

---

_Comment by @carljm on 2025-04-07 17:41_

> I do expect some difference between reveal_type and on-hover where on-hover will only show the signature (but not the type) for classes, functions, methods, and maybe more. E.g. pyright renders a function as `def foo(a: int) -> str`

This description seems backwards to me from pyright's actual behavior? For a function literal `f`, pyright renders it on-hover as "(function) def f(x: int) -> str", and on `reveal_type` gives just `(x: int) -> str`. I'm not too bothered by that minimal level of difference, though I also don't see any strong rationale _not_ to include the function name in `reveal_type` and make them even more similar. I think we should keep them identical until/unless we have strong and clear reasons to do otherwise.

> I was assuming we are just changing from `Literal[some_type.some_bound_symbol]` to `(args...) -> return_ty` for `reveal_type` and by extension LSP.

I would be inclined to include the function name for a function literal (as that's one of the few things that distinguishes it from a general callable type), so more like `f(x: int) -> str`. But otherwise yes, your assumption matches my intention in filing this issue.

> Yes. But I've already done such things, I am fine doing this as long as we quickly merge a PR, so I do not have to constantly update test files as main is moving ahead.

Thank you, and makes sense. The one thing I would suggest is to first push just the change without the mdtest updates and make sure we are all happy with how the new format looks in all tested cases (with the new inline mdtest failures on CI, it will be easy to see the differences even if mdtests are failing). Once we are confirmed happy with the new format, then update the mdtests and land immediately.

---

_Closed by @charliermarsh on 2025-05-07 00:11_

---
