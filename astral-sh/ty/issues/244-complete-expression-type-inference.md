```yaml
number: 244
title: complete expression type inference
type: issue
state: closed
author: carljm
labels:
  - generics
  - runtime semantics
assignees: []
created_at: 2024-08-05T22:22:28Z
updated_at: 2025-05-30T00:36:03Z
url: https://github.com/astral-sh/ty/issues/244
synced_at: 2026-01-12T15:54:22Z
```

# complete expression type inference

---

_@carljm_

- [x] number literal expression
- [x] string literal expression
- [x] bytes literal expression
- [x] f-string expression
- [x] ellipsis literal expression
- [x] call expression
- [x] unary expression
- [x] binary expression
- [x] boolean expression
- [x] astral-sh/ruff#13618
- [x] astral-sh/ruff#13689
- [x] astral-sh/ruff#13853
- [x] lambda expression
- [ ] generator expression (blocked on generics)
- [ ] list comprehension (blocked on generics)
- [ ] dict comprehension (blocked on generics)
- [ ] set comprehension (blocked on generics)
- [ ] starred expression (should be done as part of args/kwargs support)
- [ ] yield expression (part of async)
- [ ] yield-from expression (part of async)
- [ ] await expression (part of async)


---

_Label `help wanted` added by @carljm on 2024-09-08 03:26_

---

_Comment by @mdbernard on 2024-09-16 07:08_

Could you please clarify why some of these are currently blocked, and what actions would be needed to unblock them?

---

_Comment by @carljm on 2024-09-16 12:52_

> Could you please clarify why some of these are currently blocked, and what actions would be needed to unblock them?

Hi! Thanks for the nudge. Some of these are actually no longer blocked, because we already did the blocking thing! I've updated the list accordingly. 

Some items are blocked by settling on a representation for generic types and handling of type variables in inference. I'm hoping to get to this in the next few weeks. 

Some others aren't really blocked but are part of a larger project (for example, adding support for async Python and awaitables.)

If there are any particular items you're interested in working on, let me know and I'm happy to discuss in more detail. 

---

_Comment by @TomerBin on 2024-09-25 05:34_

Boolean expression can now be checked off ✅

---

_Comment by @cake-monotone on 2024-10-06 06:20_

Would it be okay if I worked on the lambda expression? It doesn't seem to have any major blocking issues, and I think it would be a good starting point for my first contribution to red-knot. Please let me know if you have any other suggestions.

---

_Comment by @carljm on 2024-10-07 16:22_

> Would it be okay if I worked on the lambda expression? It doesn't seem to have any major blocking issues, and I think it would be a good starting point for my first contribution to red-knot. Please let me know if you have any other suggestions.

Thanks for your interest in contributing! Lambda expression inference would require adding a general `Callable` type, which we don't have yet, and is quite complex to implement; probably not a great first contribution. I think probably a good starter task would be fleshing out some missing support in unary/binary/compare expressions; see also https://github.com/astral-sh/ruff/issues/13618

---

_Comment by @cake-monotone on 2024-10-09 08:37_

> Thanks for your interest in contributing! Lambda expression inference would require adding a general `Callable` type, which we don't have yet, and is quite complex to implement; probably not a great first contribution. I think probably a good starter task would be fleshing out some missing support in unary/binary/compare expressions; see also astral-sh/ruff#13618

Thank you for the helpful explanation! I’ll follow your suggestion and try working on the comparison task first :)

---

_Comment by @michelkluger on 2025-01-14 22:48_

> * [x]  number literal expression[x]  string literal expression[x]  bytes literal expression[x]  f-string expression[x]  ellipsis literal expression[x]  call expression[x]  unary expression[x]  binary expression[x]  boolean expression[x]  [[red-knot] Compare expressions #13618](https://github.com/astral-sh/ruff/issues/13618)[x]  [[red-knot] Infer subscript expression types #13689](https://github.com/astral-sh/ruff/issues/13689)[x]  [[red-knot] Infer slice expression types #13853](https://github.com/astral-sh/ruff/issues/13853)[ ]  generator expression (blocked on generics)[ ]  list comprehension (blocked on generics)[ ]  dict comprehension (blocked on generics)[ ]  set comprehension (blocked on generics)[ ]  lambda expression (blocked on Callable type)[ ]  starred expression (should be done as part of args/kwargs support)[ ]  yield expression (part of async)[ ]  yield-from expression (part of async)[ ]  await expression (part of async)

is this list up to date? how is the progress of red knot?

thanks in advance

---

_Comment by @carljm on 2025-01-14 23:03_

List is accurate. Progress is we are working on it and there's still a lot to do :)

---

_Label `help wanted` removed by @carljm on 2025-03-15 16:22_

---

_Renamed from "[red-knot] complete expression type inference" to "complete expression type inference" by @MichaReiser on 2025-05-07 15:27_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:17_

---

_Label `generics` added by @AlexWaygood on 2025-05-11 07:17_

---

_Comment by @carljm on 2025-05-30 00:36_

Everything remaining here is now covered by more specific issues: #543, #151, #191, #247. Closing this in favor of those issues.

---

_Closed by @carljm on 2025-05-30 00:36_

---
