```yaml
number: 529
title: Support or work-around for alternative base class declaration
type: issue
state: open
author: dimaqq
labels:
  - question
  - set-theoretic types
assignees: []
created_at: 2025-05-28T05:55:33Z
updated_at: 2025-11-18T16:10:29Z
url: https://github.com/astral-sh/ty/issues/529
synced_at: 2026-01-12T15:54:23Z
```

# Support or work-around for alternative base class declaration

---

_@dimaqq_

### Question

We've got some code that has to support both pydantic 1 and 2 for historical reasons.

It looks like this:

```py
PYDANTIC_IS_V1 = int(pydantic.version.VERSION.split(".")[0]) < 2
if PYDANTIC_IS_V1:
    class DatabagModel(BaseModel):  # type: ignore
        ...
else:
    class DatabagModel(BaseModel):
        ...
```

Attempting to use this model is rejected by `ty`:

```py
warning[unsupported-base]: Unsupported class base with type `<class 'DatabagModel'> | <class 'DatabagModel'>`
   --> lib/charms/traefik_k8s/v2/ingress.py:238:30
    |
238 | class IngressRequirerAppData(DatabagModel):
    |                              ^^^^^^^^^^^^
```



### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Label `question` added by @dimaqq on 2025-05-28 05:55_

---

_Label `set-theoretic types` added by @sharkdp on 2025-05-28 06:47_

---

_Comment by @sharkdp on 2025-05-28 06:53_

Thank you for reporting this.

We currently do not support union types as class bases. It is possible that we will support that at some point, but it's probably not a high-priority feature.

As a work-around, is there a way in which you could make `PYDANTIC_IS_V1` a statically known constant (by making that version check easier to understand)? I understand that you probably have no control over `pydantic.version.VERSION`, but if we can infer the type of `PYDANTIC_IS_V1` to be `Literal[True]` or `Literal[False]`, we should be able to understand that `DatabagModel` is just a single class type. For example:

https://play.ty.dev/503b8c67-a294-4c25-a1bb-463eb7e4d0bf
```py
VERSION = (2, 1, 3)

PYDANTIC_IS_V1 = VERSION[0] < 1

if PYDANTIC_IS_V1:
    class DatabagModel:
        ...
else:
    class DatabagModel:
        ...

class IngressRequirerAppData(DatabagModel):
    pass
```

---

_Comment by @MichaReiser on 2025-05-28 07:01_

Could another alternative be to gate individual members by the pydantic version check instead of the entire class?

```py
PYDANTIC_IS_V1 = int(pydantic.version.VERSION.split(".")[0]) < 2
class DatabagModel(BaseModel):  # type: ignore
	if PYDANTIC_IS_V1:
        ...
	else:
        ...
```


---

_Comment by @sharkdp on 2025-05-28 07:12_

Wondering if we could make things like this easier by providing extended type inference for patterns that would appear in such version checks. For example, if `VERSION` is a literal string ("2.11.5"), like in this case, we could potentially infer a "LiteralList" type for `VERSION.split(".")`. Something like `LiteralList[Literal["2"], Literal["11"], Literal["5"]]`. From there, we could support index-access with literal integers on those `LiteralList` types, so `VERSION.split(".")[0]` would become `Literal["2"]`. And finally, we would also have to support `int(…)` calls for literal strings. None of which sounds completely unreasonable, especially if we want to introduce a `LiteralList` type for other reasons as well.

~~Another thing that came to my mind was to add type inference for `hasattr`. We already support narrowing for `hasattr`, and it should be easy to also infer `Literal[True] / Literal[False] / bool` for `hasattr` calls, depending on whether the attribute is bound, unbound, or possibly-bound. This way, users could detect library versions by checking for attributes that are only available in one version: `PYDANTIC_IS_V1 = not hasattr(pydantic, "some_api_only_available_in_v2")`.~~ (Edit: this seems like a bad idea, see https://github.com/astral-sh/ruff/pull/18348#issuecomment-2915390051)

---

_Comment by @carljm on 2025-05-28 17:53_

@dimaqq Are you currently successfully type-checking this code with another type checker? On a simple version of this code, it seems like both [pyright](https://pyright-play.net/?strict=true&code=CYUwZgBA5iAuD6AjA9sgNgCgJQQLQD4IV0AuCAOkoCgqAFATQBEBBAOQBUBJAYXk4GV4ANQCMEALzQ4SVJiw0AlpAYsOPPoNEkqEXRADGaAIYBnExEZHYRxEagBZZKDTa9bitRBoTIV3sOm5pbWtg5OXn7uHuQ0AWYQnAB2UABOIGYASiAAjgCuCmkpzAAOxcFGGOWhjs5YkRDFgUA) and [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=e236a8d967a0597f88d84dee4788e83d) also fail (although they error on the repeat definition of a same-named class in the same scope, rather than on inheriting from a union.)

It does seem like some extended inference for `.split` on a `LiteralString` could help here. A simpler workaround that could work with ty today would be to use string indexing instead (`VERSION[0] == "1"`), though that's obviously less robust than using `.split` and `int()`.

The other issue here is that `pydantic.version.VERSION` is not annotated, so we actually infer `Unknown | Literal["2.11.5"]` for it, which would break all of these solutions. Ideally pydantic would annotate it at least as `: Final`, which should cause us to remove the `Unknown |` (once we support `Final`).

---

_Comment by @dimaqq on 2025-06-05 05:53_

That's good question as I was running on ty on someone else's codebase.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:37_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
