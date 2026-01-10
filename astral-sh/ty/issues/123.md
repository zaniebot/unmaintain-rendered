```yaml
number: 123
title: possible cycle with class inheritance and visibility constraints in a stub file
type: issue
state: closed
author: carljm
labels: []
assignees: []
created_at: 2025-04-18T19:50:34Z
updated_at: 2025-05-11T07:36:29Z
url: https://github.com/astral-sh/ty/issues/123
synced_at: 2026-01-10T02:34:09Z
```

# possible cycle with class inheritance and visibility constraints in a stub file

---

_Issue opened by @carljm on 2025-04-18 19:50_

This code, in a stub file, results in a query cycle, with `ClassLiteralType::try_metaclass` as the cycle head:

```py
class A:
    def lucky_number(self): ...

class B(A):
    def lucky_number(self): ...

obj = B()
assert obj.lucky_number() == 2
```

The issue is that name resolution of `A` in `class B(A)` is deferred (because we are in a stub file), and the public binding of `A` has a visibility constraint of `obj.lucky_number() == 2`. Evaluating that visibility constraint requires figuring out what `obj.lucky_number` is, which means resolving the MRO of `B`, which means resolving the public binding of `A`.

I'm recording this issue for future consideration, but not prioritizing it right now, since I think it's very unlikely to occur in the wild; since stub files are not executed, they are very unlikely to have assertions or conditionals based on the types they themselves define. (There may well be visibility constraints on e.g. `sys.version_info` or other simple constants, but those won't cause this kind of cycle.)

It may be that we find other, more compelling reasons to add fixpoint handling to `ClassLiteralType::try_metaclass`, in which case this cycle panic would go away too.



---

_Comment by @carljm on 2025-04-18 19:53_

It feels like when resolving deferred names, we really shouldn't need to take into account the possibility of the scope not completing at all. But I'm not sure how to model this exactly. Maybe by `OR`ing the visibility constraints on each binding with the negation of `scope_start_visibility`? That is, when considering the visibility of a given binding, we should eliminate from consideration those cases in which the scope-start itself is not visible.

---

_Assigned to @carljm by @carljm on 2025-04-18 19:53_

---

_Comment by @AlexWaygood on 2025-04-18 21:25_

I'd honestly be okay with just banning `assert` and `raise` statements in stub files if we can't be confident that we can handle them without panicking. I can't see any possible use case for having them in stub files. Do other type checkers accept them?

---

_Comment by @JelleZijlstra on 2025-04-18 21:34_

The only use case I can think of is platform asserts, like `assert sys.platform == "win32"` for a Windows-only stub. I feel like that's been proposed but I'm not sure any other type checker supports it.

If we add that, I'd be fine with restricting them syntactically to be before any class/function definition though, which should simplify life for you.

---

_Renamed from "[red-knot] possible cycle with class inheritance and visibility constraints in a stub file" to "possible cycle with class inheritance and visibility constraints in a stub file" by @MichaReiser on 2025-05-07 15:25_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 15:56_

---

_Comment by @AlexWaygood on 2025-05-11 07:36_

We no longer panic on this snippet in a stub file (https://play.ty.dev/da6d32c2-cf68-47ba-98fd-1e524e62ff42), presumably as a result of https://github.com/astral-sh/ruff/pull/17758

---

_Closed by @AlexWaygood on 2025-05-11 07:36_

---
