```yaml
number: 18894
title: Improve docs for UP046 regarding reusable TypeVars
type: issue
state: open
author: wardVD
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-06-23T12:30:56Z
updated_at: 2025-10-29T19:56:51Z
url: https://github.com/astral-sh/ruff/issues/18894
synced_at: 2026-01-10T11:09:59Z
```

# Improve docs for UP046 regarding reusable TypeVars

---

_Issue opened by @wardVD on 2025-06-23 12:30_

### Summary

The rule [non-pep695-generic-class](https://docs.astral.sh/ruff/rules/non-pep695-generic-class/) doesn't seem to cover a use case where you define a `TypeVar` in a module which is imported in several other files.

For example, imagine I have a file
`my_types.py`

with 
```
from typing import TypeVar

CustomNumber = TypeVar("CustomNumber", bound=int)
``` 

and two files `a.py` with

```
from mytypes import CustomNumber

class A(Generic[CustomNumber]):
    ...
```

and `b.py` with 

```
from mytypes import CustomNumber

class B(Generic[CustomNumber]):
    ...
```

If I want to change `CustomNumber` to have an upper bound of `float` I would only need to change in one location. If I would adhere to PEP695 I would need to remove the `my_types.py` file and refactor the code to e.g.

```
class A[CustomNumber: int]():
    ...
```
and
```
class B[CustomNumber: int]():
    ...
```
thus breaking code reproducibility if I would want to change the upper bound. This could lead to code inconsistencies

I deliberately made the example a bit dummy and trivial but I think it makes the issue clear :-).

---

_Comment by @ntBre on 2025-06-23 13:47_

Ah that's an interesting point! We couldn't currently handle this for multiple files since Ruff only analyzes one file at a time, but it's still relevant for the single-file case, I think: https://play.ruff.rs/c90975da-b469-4644-a4bd-b7e23c375db0

---

_Label `rule` added by @ntBre on 2025-06-23 13:47_

---

_Label `needs-decision` added by @ntBre on 2025-06-23 13:47_

---

_Label `type-inference` added by @MichaReiser on 2025-06-23 13:53_

---

_Comment by @AlexWaygood on 2025-06-23 13:53_

I think it's a feature that the rule enforces PEP-695 syntax even for cases like this. Quoting [the PEP](https://peps.python.org/pep-0695/#points-of-confusion):

> The scoping rules for type variables are difficult to understand. Type variables are typically allocated within the global scope, but their semantic meaning is valid only when used within the context of a generic class, function, or type alias. A single runtime instance of a type variable may be reused in multiple generic contexts, and it has a different semantic meaning in each of these contexts. This PEP proposes to eliminate this source of confusion by declaring type parameters at a natural place within a class, function, or type alias declaration statement.

And for similar reasons, `typing.AnyStr` (perhaps the most often used "reusable TypeVar") [is now deprecated](https://github.com/python/cpython/issues/105578). If you want to have clearly scoped TypeVars that use modern syntax, and you only support Python 3.12+, I think you should aim to use PEP-695 syntax wherever possible. 

> If I want to change `CustomNumber` to have an upper bound of `float` I would only need to change in one location.

This could possibly be solved by using a type alias? 

```py
type CustomNumberBound = int

class A[CustomNumber: CustomNumberBound]: ...
class B[CustomNumber: CustomNumberBound]: ...
```

---

_Label `type-inference` removed by @AlexWaygood on 2025-06-23 13:53_

---

_Comment by @wardVD on 2025-06-24 09:06_

@AlexWaygood That's a very good solution I didn't think about. Still getting used to the new convention. Thank you!

Then I think this issue can be closed indeed. I don't know if it would be useful to have some more documentation on examples like these to get people going. I think it's near impossible to fix these by `ruff` automatically but the code refactors can become quite extensive if we need to adhere to PEP695 and people won't do it since they don't know how from the get-go.
I'll leave it up to you guys to make a decision but can be closed for me. Thanks for the help!



---

_Comment by @AlexWaygood on 2025-06-24 10:12_

I think one thing we could definitely do here is to expand the "Why is this bad?" section in the docs to make it more obvious that the clearer scoping of the TypeVar is part of the reason why you should prefer the new syntax. The full quote I pasted above might be a bit long to include verbatim, but we could include something like it and possibly link to the PEP.

I also wouldn't be opposed to adding another example to the `## Examples` section, such as

````md
```py
from typing import TypeVar, Generic

ReusableT = TypeVar("ReusableT", bound=int | str | dict[int, str])


class GenericClass1(Generic[ReusableT]):
class GenericClass2(Generic[ReusableT]): ...
class GenericClass3(Generic[ReusableT]): ...
```

Use instead:

```py
type ReusableTBound = int | str | dict[int, str]

class GenericClass1[ReusableT: ReusableTBound]:
class GenericClass2[ReusableT: ReusableTBound]: ...
class GenericClass3[ReusableT: ReusableTBound]: ...
```
````

Now that I look at it, I see the existing example is also missing an import of `Generic` at the top!

---

_Label `documentation` added by @AlexWaygood on 2025-06-24 10:12_

---

_Label `rule` removed by @AlexWaygood on 2025-06-24 10:12_

---

_Label `needs-decision` removed by @AlexWaygood on 2025-06-24 10:12_

---

_Label `help wanted` added by @MichaReiser on 2025-06-24 12:20_

---

_Renamed from "Extend rule UP046 with reusable TypeVars" to "Improve docs for UP046 regarding reusable TypeVars" by @AlexWaygood on 2025-06-24 21:19_

---

_Comment by @jonaslb on 2025-10-29 12:30_

Something I just encountered is - I had put explicit variance (covariant/contravariant) on some type parameters. I'm aware that this is inferred with the new syntax. However, I _wanted_ a particular variance. I don't want future edits to accidentally change the variance (eg from covariant to invariant).

Does someone know if

- Specifying variance explicitly is considered deprecated as well?
- Or if it's still "considered a reasonable thing to do", in which case this lint should probably not trigger for TypeVars with variance specified?

---

_Comment by @ntBre on 2025-10-29 13:35_

@jonaslb There's a related issue about variance in https://github.com/astral-sh/ruff/issues/18976. I think Alex's comment [here](https://github.com/astral-sh/ruff/issues/18976#issuecomment-3013154682) may help to answer your question.

> In most cases, type checkers should be able to correctly infer the variance of a TypeVar (that's why it's not possible to manually specify them with the new syntax, after all!). So it should be pretty rare that you need to manually override the type checker's inference and specify a different variance to the one that the type checker infers. I'd argue it's rare enough that you should just be able to deal with this by adding # noqa comments to the specific problematic classes?

---

_Comment by @jonaslb on 2025-10-29 19:56_

I'm perfectly happy with the quality of the inference. The trouble I feared was that another developer could make changes that'd accidentally change the inferred variance. Not because the inference was wrong, but because a lot of people don't have a good understanding of variance. 

What I did earlier was try to convince myself it was unlikely to happen, or that we would have type checks of other parts of the codebase failing if someone was going to try to do something like that. And hence simply embrace the inference - it does make the code simpler, after all.

---
