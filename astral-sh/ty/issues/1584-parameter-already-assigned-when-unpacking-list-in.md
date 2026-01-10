---
number: 1584
title: parameter-already-assigned when unpacking list in function call
type: issue
state: open
author: Castavo
labels:
  - calls
assignees: []
created_at: 2025-11-18T15:19:25Z
updated_at: 2026-01-09T11:14:21Z
url: https://github.com/astral-sh/ty/issues/1584
synced_at: 2026-01-10T01:48:23Z
---

# parameter-already-assigned when unpacking list in function call

---

_Issue opened by @Castavo on 2025-11-18 15:19_

### Summary

When unpacking a list inside a function call, it seems as though ty interprets it as filling up all arguments available in the function.

The following : 
```py
def test(l1: str, l2: str, other: float):
    ...

test(*["a", "b"], other=0.1)
```
(Also found in [playground](https://play.ty.dev/a20f2bb1-e1bf-4556-aa4a-6bc8a49baf46))

Gives both `invalid-argument-type` and `parameter-already-assigned`.

I would have expected ty to be more lenient here: trust the keyword assignment of `other`, and then fill the rest of the arguments with the unpacked list

### Version

7a739d6b7

---

_Label `calls` added by @carljm on 2025-11-18 16:08_

---

_Comment by @carljm on 2025-11-18 16:08_

Thanks for the report! This is a bit tricky, as it seems like the only heuristic available is "try different expansions and see what works". It looks like our current behavior [matches mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=3c3caa77b6362b2f5325a1cc91e3b897) and [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSImMYABAC7x0AUUAjIjXHQE4A0NUAEwcufGrjoALGNw5gouVHQCUiADroaWmoV0b96BlyYAqANpqQqS-0vZLAXX4Tp3ALwAGQq2UheIAFc6aDgSckQQAGIaAFVgqAg6UhowAPQAY2DcdDgDKlowXG4AWyUAfXQA4uwZJnwOCENlGgBaAD5OHnVNbW4YOgDuTTBLADkqmtkaYHwAX0sNPxAyPvlSQjpcYqgKaIAFUlWoZLQsPHwadOzIAHNBpQhswg1ogGUYGBpJOjpiOEQAPQAlbUY6EIo3AEwdAAzC4dJwAFXdC3e5ZGEpIo0VAAN1Q0FQ2Fgl2uEDu3Ae2XExHRoQ0ZCk2RaOJkcEemjcNEsAGZvIIFugQLN-KhMhAWQAxaAwCinHAEMJCoA), but [pyright does better](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.9&reportUnreachable=true&code=CYUwZgBALiDOUAoA2BGAXBeAnANBJATBtngPZQAWIWGYSpAhlAJRoBQEnEAdL2-zHgIAVAG0ARA3F5xAI3EBdMpWoBeAAzcUzNkA).

We do better with [a tuple unpacking](https://play.ty.dev/10b1d57f-2d7d-4391-8e28-fc03f872b2c0), where the length is tracked.

---

_Comment by @carljm on 2025-11-18 16:46_

> the only heuristic available is "try different expansions and see what works".

Actually this isn't right; I was thinking the heuristic needed to be based on the parameter annotations and assignability of argument types, but in this case we could do better simply by avoiding double-assigning an argument to a parameter. That seems quite doable.

---

_Added to milestone `Stable` by @carljm on 2025-11-20 00:12_

---

_Comment by @wes-sleeman on 2025-11-21 20:42_

Thought: Would it be possible to add an `Option<int>` to the representation of the list type (along with the generic parameter) which is `Some(length)` if a fixed length is known and `None` if the length isn't known? That would help with things like this without adding _too_ much extra tracking burden… The current behaviour could be maintained if the length is `None`, but if a known length is present then much better tuple-style handling could be performed.

Possible edge case handling:
* Passing a list across a function boundary (arg or return) would automatically scrub the length field.
* `append`/`pop`/etc. can be handled cleverly by modifying the length field.
* Anything hit that we haven't thought of can be a `length = None` with a TODO as it would be "no worse" than current behaviour.

---

_Comment by @carljm on 2025-11-21 20:49_

@wes-sleeman Lists are currently handled in ty (and I think in other Python type checkers too?) entirely via the generalized handling for generic types that is available for user-defined types as well. So there is no "representation of the list type" per se -- or rather, the representation is a very general one that is the same as the representation of any instance of any user-defined class (based entirely on the definition of `class list` in typeshed). So adding this kind of logic is not just a quick addition to an existing special-cased "list" type; it would be adding an entirely new system for special-cased handling of lists.

---

_Comment by @carljm on 2025-11-21 21:07_

It is plausible that we might add some generalized "length narrowing" support that could potentially be applied to any class with `__len__` (https://github.com/astral-sh/ty/issues/560#issuecomment-3013041690 describes a plan for this); then it's additionally plausible that we could add such "narrowing" information to a newly-created list literal.

But I think this is not really necessary in order to fix the case in the OP here; all we need is smarter argument-matching that avoids creating "multiple arguments assigned to parameter" errors when they are easily avoidable by adjusting the unpacking.

---

_Comment by @wes-sleeman on 2025-11-21 22:47_

@carljm Ah, okay. I was looking at the [ListExpr in Ruff's AST](https://github.com/astral-sh/ruff/blob/ddc1417f22599ceaa998c1ee4053c657afb2d577/crates/ruff_python_ast/src/generated.rs#L9588-L9596) and thinking we might be able to add something in there (like how it's got the elts or how the tuple tracks if it's parenthesised) and thread it through. I assume it's not as simple as putting a len check on the correct `parse_list_*` call? I'll take a deeper look into how it's handled for tups as I'm pretty new to this codebase and am still struggling a bit to figure out what code runs when. :)

---

_Comment by @carljm on 2025-11-22 02:32_

Determining the length of a list literal in the AST is not difficult; we don't need an extra field for that, we can just count the elements. Representation of a list literal in the AST is not the same as the representation of an arbitrary list type in the type checker (in `crates/ty_python_semantic`).

---

_Comment by @Boon-in-Oz on 2026-01-07 00:28_

Can I assume a fix for this would also fix `No overload of bound method '__init__' matches arguments` for the following?
```
data = [1, 2, 3]
test = QtGui.QColor(*data, 127)
```
where one of the overloads for QColor in the type stubs is
```
@typing.overload
def __init__(self, r: int, g: int, b: int, a: int = ...) -> None: ...
```

---

_Comment by @carljm on 2026-01-07 00:31_

@Boon-in-Oz seems quite likely, yes

---

_Removed from milestone `Stable` by @carljm on 2026-01-07 00:31_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-07 00:31_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-09 11:14_

---
