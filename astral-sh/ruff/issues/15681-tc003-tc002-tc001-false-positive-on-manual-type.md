```yaml
number: 15681
title: "TC003/TC002/TC001 False positive on manual `TYPE_CHECKING` with `__future__.annotations`"
type: issue
state: closed
author: cibere
labels:
  - rule
assignees: []
created_at: 2025-01-23T02:21:47Z
updated_at: 2025-02-06T13:45:17Z
url: https://github.com/astral-sh/ruff/issues/15681
synced_at: 2026-01-12T15:54:54Z
```

# TC003/TC002/TC001 False positive on manual `TYPE_CHECKING` with `__future__.annotations`

---

_@cibere_

### Description

If you manually set `TYPE_CHECKING` to `False` and use it with `__future__.annotations`, ruff will raise TC003/TC002/TC001 falsepositives on the imports in your `TYPE_CHECKING` block as it seems to only check for `typing.TYPE_CHECKING`.

Command: `ruff check test.py --isolated --fix --select TC`
Version: `0.9.2`
Keywords: "TYPE_CHECKING", "TC003", "__future__.annotations", "TC002", "TC001"

# Sample Code
`test.py`
```py
from __future__ import annotations

TYPE_CHECKING = False
if TYPE_CHECKING:
    from types import TracebackType

def foo(tb: TracebackType):...
```
```sh
$ ruff check test.py --isolated --fix --select TC
test.py:5:23: TC003 Move standard library import `types.TracebackType` into a type-checking block
  |
3 | TYPE_CHECKING = False
4 | if TYPE_CHECKING:
5 |     from types import TracebackType
  |                       ^^^^^^^^^^^^^ TC003
6 |
7 | def foo(tb: TracebackType):...
  |
  = help: Move into type-checking block

Found 1 error.
```

---

_Renamed from "TC003 Falsepositive on manual TYPE_CHECKING with __future__.annotations" to "TC003/TC002/TC001 Falsepositive on manual TYPE_CHECKING with __future__.annotations" by @cibere on 2025-01-23 04:20_

---

_Comment by @Daverball on 2025-01-23 07:14_

In the original flake8 plugin I treat all `ast.Name` loads with the id `TYPE_CHECKING` as implicitly `typing.TYPE_CHECKING`. This would allow this to work, but it can lead to inaccurate behavior the other way (although I don't think it's likely to ever cause any issues in real code, that doesn't explicitly try to trick us).

We could be more narrow and just detect this specific case, where in addition to the above condition, if the qualified name doesn't match a typing module, `resolve_assignment` on the `ast.Name` must result in `builtins.False`. It is a slightly more expensive check (although not that much more expensive either, considering there's usually only one of these in a file, unless you make heavy use of mixins).

---

_Comment by @Daverball on 2025-01-23 07:26_

The only other thing I'll say, is that the fixes for the TC range currently don't support this "avoid importing `typing` at runtime entirely" pattern currently. So this is something we may want to give people as an option (maybe we just switch to this style as soon as `typing` isn't part of `exempt_modules`)?

I.e. when the `typing` module isn't exempt and no `typing` imports other than `TYPE_CHECKING` are used at runtime, it will add `TYPE_CHECKING = False` above the first block in global scope and move the typing import inside. But there's a lot of potential complexities here, that could make this difficult, so maybe we'll just let people handle these cases on their own.

The other caveat is, that the fix for TC005 would need to remove `TYPE_CHECKING = False` in addition to the empty type checking block, if there are no other references to `TYPE_CHECKING` in that file.

---

It's also worth noting that ruff supports `if False` and `if 0`, but newer versions of type checkers no longer support this, since it was taken out of the spec. There have been recent discussions about putting something back into the spec that allows getting rid of the runtime `typing` import. While this pattern works with `mypy` and `pyright`, it's not part of the typing spec and I'm not sure if other type checkers recognize it.

---

_Label `rule` added by @MichaReiser on 2025-01-23 07:43_

---

_Comment by @MichaReiser on 2025-01-23 07:44_

@carljm @AlexWaygood what are our plans for Red Knot around supporting non-`typing` `TYPE_CHECKING` guards? Is this something we plan on supporting and if not, why?

---

_Comment by @Daverball on 2025-01-23 08:09_

The recent discussion I talked about can be found here: https://discuss.python.org/t/specify-type-checking-false-without-typing-import/76766

---

_Comment by @dhruvmanila on 2025-01-23 09:02_

FWIW, both mypy and Pyright seems to support non-`typing` `TYPE_CHECKING` guards:
* mypy: https://mypy-play.net/?mypy=master&python=3.10&gist=d1986f8345de346c91188f530b71a3e9&flags=show-traceback
* Pyright: https://pyright-play.net/?pythonVersion=3.10&strict=true&code=CoTQCgog+gwgEhGBpAkgOQOIAIC8WBiAhgDYDOApgFCUCWAZlqJLAsuhgFyVY9Z0BOAewC2WAMaDixcmIAuNQQDtSAOkIAjMVhrCADoP6ysAWUK7dNRQHNuvagBMOJsxesBtUrP4AaLJ-4AurhYAN4AvpRAA

---

_Comment by @Daverball on 2025-01-23 10:34_

> FWIW, both mypy and Pyright seems to support non-typing TYPE_CHECKING guards:

Yup, although if you look at the linked discussion, Eric opposes the addition of this form to the typing spec, so I'm guessing the only reason pyright currently supports it, is so he can offer better feature parity with mypy. The discussion didn't really reach a consensus about how to support this use-case.

Although given, that mypy and pyright have the biggest install bases currently, I don't see a strong reason for ruff to hold out on support for this, even if it ends up being phased out again, like the previous `if False` support in type checkers, which was used for supporting versions of Python, that did not yet have a `typing` module.

I personally think `if False:` paired with a pragma style comment would offer the best UX, beyond adding `TYPE_CHECKING` to `builtins` (which would only really help with new Python versions going forward), but that's besides the point, until there's actually something in the typing spec, we can rely on.

---

_Comment by @carljm on 2025-01-23 15:47_

> what are our plans for Red Knot around supporting non-`typing` `TYPE_CHECKING` guards?

I'm not sure. My vague memory is that I had commented somewhere that we should support this, and there was some pushback (I don't recall from who) so we deferred it. I can't seem to find that discussion now.

I don't have strong feelings in either direction, but my inclination is that we should support it in order to lower migration barriers from mypy/pyright, because I don't think there is much cost in supporting it.

---

_Renamed from "TC003/TC002/TC001 Falsepositive on manual TYPE_CHECKING with __future__.annotations" to "TC003/TC002/TC001 False positive on manual `TYPE_CHECKING` with `__future__.annotations`" by @dylwil3 on 2025-01-23 23:26_

---

_Comment by @sharkdp on 2025-01-24 09:37_

> > what are our plans for Red Knot around supporting non-`typing` `TYPE_CHECKING` guards?
> 
> I'm not sure. My vague memory is that I had commented somewhere that we should support this, and there was some pushback (I don't recall from who) so we deferred it. I can't seem to find that discussion now.

This one? https://github.com/astral-sh/ruff/pull/14759#discussion_r1876804007

---

_Comment by @AlexWaygood on 2025-01-24 09:45_

> > > what are our plans for Red Knot around supporting non-`typing` `TYPE_CHECKING` guards?
> > 
> > 
> > I'm not sure. My vague memory is that I had commented somewhere that we should support this, and there was some pushback (I don't recall from who) so we deferred it. I can't seem to find that discussion now.
> 
> This one? [#14759 (comment)](https://github.com/astral-sh/ruff/pull/14759#discussion_r1876804007)

I tried to find that comment thread yesterday but failed — thanks @sharkdp!

I still have the same opinion as I did then, that treating _all_ variables named `TYPE_CHECKING` as always truthy doesn't feel very principled, and that it feels like it might be redundant once we have a generalised `--always-truthy` setting for specifying constants that red-knot should consider to be always truthy or always falsey. That said, I don't have a strong opinion on this. I'm okay with doing the same as mypy and pyright if it turns out to be very painful or unintuitive for users that we have different behaviour to the other type checkers here.

---

_Comment by @Daverball on 2025-01-24 10:25_

> [...] once we have a generalised --always-truthy setting for specifying constants that red-knot should consider to be always truthy or always falsey.

Unfortunately that's not really a viable solution for library authors. You can't really ask that all your downstream users adjust their type checker configuration for your special `TYPE_CHECKING` constant. It's only really a viable solution, if your code has no downstream consumers, or you are the only downstream consumer.

The use-case is pretty clear. People want there to be a portable way for not making every downstream user pay the cost of the `typing` import. Remember that not every user of a typed library will actually make use of static type checking and even when they do, they might operate under tight startup time constraints, so every import they can shave off matters, especially expensive ones like `typing`.

While shipping separate stub files achieves that goal, it can be a pain to maintain. So I have sympathy for people that want there to be a standard way, to do the `if TYPE_CHECKING` check without having to import `typing`.

I do however agree with you, that this solution feels icky (especially since it messes with runtime type checking) and I think we can provide a better solution eventually, where we don't have to rely on `TYPE_CHECKING` blocks, like e.g. providing a new type import statement, that defers the import until the symbol is explicitly accessed.

But until we have a better solution, it would be nice to give people a way that's at least supported by the most popular type checkers right now, even if it's not part of the typing spec.

---

_Comment by @carljm on 2025-01-24 14:44_

I agree with @Daverball here. I agree that in principle it's icky to special case a variable name, but I think the practical harm is very small, the use case is real, and the version that requires explicit configuration is not usable for libraries. So I would support exploring standardizing nicer ways to do this, but pending that I think we should match mypy and pyright. 

---

_Comment by @AlexWaygood on 2025-01-24 14:46_

Yeah, that all seems reasonable to me; I'm happy to go with that. I agree that @Daverball makes a really good point about libraries.

---

_Comment by @AlexWaygood on 2025-01-24 16:38_

We've opened an issue to track changing red-knot's behaviour to match other type checkers: https://github.com/astral-sh/ruff/issues/15722 (thanks @carljm!)

---

_Closed by @MichaReiser on 2025-02-06 13:45_

---

_Closed by @MichaReiser on 2025-02-06 13:45_

---
