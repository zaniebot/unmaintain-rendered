```yaml
number: 1706
title: Accessing a class attribute via an instance produces Unknown type
type: issue
state: closed
author: alex
labels:
  - question
  - attribute access
assignees: []
created_at: 2025-12-01T13:47:29Z
updated_at: 2025-12-01T21:33:22Z
url: https://github.com/astral-sh/ty/issues/1706
synced_at: 2026-01-10T01:58:59Z
```

# Accessing a class attribute via an instance produces Unknown type

---

_Issue opened by @alex on 2025-12-01 13:47_

### Summary

Minimal reproducer: https://play.ty.dev/ee9d00f3-683c-4ab6-8a10-b2494da94501

```
import typing

BAR: typing.Any = None

class Binding:
    ffi = BAR

    def __init__(self) -> None:
        typing.reveal_type(self.ffi)
```

For comparison with mypy: https://mypy-play.net/?mypy=latest&python=3.12&gist=ae1fd0175a91a34e7106fe67ca405d04

(Apologies if there's an existing bug for this, I attempted to search but didn't see anything.)

### Version

0e651b50b (playground)

---

_Label `question` added by @sharkdp on 2025-12-01 18:06_

---

_Label `attribute access` added by @sharkdp on 2025-12-01 18:06_

---

_Comment by @sharkdp on 2025-12-01 18:16_

Thank you for reporting this.

ty distinguishes between the declared type, and the control-flow-sensitive (local) inferred type. Here, the inferred type for `BAR` is `None`, and since the class body is eagerly executed, we know that it's still `None` when `ffi = BAR` is executed. Since the `ffi` attribute is not declared, we union with `Unknown` (see [this document](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md) for an explanation why).

So this is pretty much the intended behavior at the moment, but we do realize that it is confusing to seemingly ignore the declared type. See #136 for a related discussion.

(Even if we prefer the declared type of `BAR` here, though, `ffi` is still un-annotated and not declared anywhere. If you want to enforce an attribute type of `Any`, that attribute will probably have to be annotated/declared explicitly) 

---

_Comment by @alex on 2025-12-01 21:19_

 I think I understand the theory here -- at least as to `Unknown` on all public APIs. However, the user experience is fairly confusing, to make this practical I think `ty` needs some sort of "type coverage" mode that tells me anyplace `| Unknown` is getting added to my types.

And as for #136, using something other than my declared types is fairly confusing and seems to risk leaking implementation details -- which is precisely what I understand the `| Unknown` behavior to be helping avoid.

---

_Comment by @carljm on 2025-12-01 21:25_

> using something other than my declared types is fairly confusing

At risk of straying into territory that better belongs in https://github.com/astral-sh/ty/issues/136, the problem here is that in other cases just using the declared type is confusing/wrong! With `x: int | None = 3`, people definitely expect us to locally use the inferred type of the RHS (which cannot be `None`) until/unless we see `None` assigned, rather than just the declared type `int | None`. So we need to sort out whether there is any principled behavior here that will also give the results people expect, or whether the only option is some sort of heuristic (e.g. "use the declared type but filter union types specifically").

> the user experience is fairly confusing, to make this practical I think `ty` needs some sort of "type coverage" mode that tells me anyplace `| Unknown` is getting added to my types.

Yeah, I agree with this.

---

_Comment by @AlexWaygood on 2025-12-01 21:27_

> With `x: int | None = 3`, people definitely expect us to locally use the inferred type of the RHS (which cannot be `None`) until/unless we see `None` assigned, rather than just the declared type `int | None`.

well, [mypy users won't necessarily expect that](https://mypy-play.net/?mypy=latest&python=3.12&gist=249eb5d90455778220bf82539d1f6aa1). But I do agree that refusing to narrow on the initial assignment (and yet narrowing in all other situations) can be viewed as inconsistent.

---

_Comment by @carljm on 2025-12-01 21:33_

Thanks for the report! Ultimately I think this example covers territory that is covered in #136 (annotations of `Any` are discussed extensively as an example case there) and #1240, so I'll close this as duplicate (of the latter, semi-arbitrarily).

---

_Closed by @carljm on 2025-12-01 21:33_

---
