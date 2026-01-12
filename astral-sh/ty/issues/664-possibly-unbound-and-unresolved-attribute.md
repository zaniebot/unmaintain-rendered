```yaml
number: 664
title: possibly unbound and unresolved attribute diagnostic when storing attributes
type: issue
state: closed
author: Glyphack
labels:
  - bug
  - attribute access
assignees: []
created_at: 2025-06-16T19:02:54Z
updated_at: 2025-11-11T22:45:35Z
url: https://github.com/astral-sh/ty/issues/664
synced_at: 2026-01-12T15:54:23Z
```

# possibly unbound and unresolved attribute diagnostic when storing attributes

---

_@Glyphack_

### Summary

Ty emits "possibly-unbound-attribute" diagnostic on setting the attribute.

```py
from typing import Self

class Foo:
    def __init(self: Self, flag: bool):
        if flag:
            self.x = 1 # error possibly-unbound-attribute
```
https://play.ty.dev/617d2c31-fe1b-4c8a-9787-7b7baf1f886d


Currently in a class body if an instance attribute is conditionally defined or undefined, we emit a diagnostic on setting the attribute.

Instead ty should report when accessing the attribute. In the previous code example if we only accessed the `self.x` when it's undefined the diagnostic made sense.

Another example of where this diagnostic makes sense.

```py
class C:
    if flag:
        x: int

C().x  # error: [possibly-unbound-attribute]
```

comments from @carljm:
> I think that in the case of a possibly-unbound implicit instance attribute, we should never emit a diagnostic on that, whether inside or outside the class body, and whether setting or accessing the attribute

comments from @sharkdp on implementation:
> I agree that we shouldn't even attempt to emit that diagnostic in case that it refers to an instance attribute. For other attribute accesses, like on class bodies or modules, it probably still makes sense, but might be similarly problematic to possibly-unresolved-reference, which we've recently degraded to ignored-by-default.
> Fixing that would require a richer return type for Type::member, I think. We are planning to do that anyway for better error messages, but I guess in the non-error case, we could also return additional metadata (what kind of attribute is this, is the returned type the result of a descriptor __get__ method, etc.)


### Version

373a3bfcd

---

_Label `bug` added by @sharkdp on 2025-06-16 20:20_

---

_Label `attribute access` added by @sharkdp on 2025-06-16 20:20_

---

_Comment by @sharkdp on 2025-10-07 08:55_

The examples in this ticket do not emit errors anymore. This might be due to our recent changes to `Self` type variables? The errors still appear on https://github.com/astral-sh/ruff/pull/18473, but they probably need to be fixed on that branch.

---

_Closed by @sharkdp on 2025-10-07 08:55_

---

_Comment by @sharkdp on 2025-10-07 09:18_

Actually, that's not true. This example still leads to the same error:
```py
from typing import Self

class C:
    def __init__(self: Self) -> None:
        [... for self.a in range(10)]
```
https://play.ty.dev/22a5c060-38f5-434c-a6b9-9cdc467d55bb

---

_Reopened by @sharkdp on 2025-10-07 09:18_

---

_Comment by @sharkdp on 2025-10-07 10:00_

It fails for comprehensions because the definition of `self.a` is within the (eagerly evaluated) comprehension scope, and our detection of potential instance attributes by checking for methods in classes [here](https://github.com/astral-sh/ruff/blob/15af4c0a34dd6d86d9aa98115ae1e1e9392c7eef/crates/ty_python_semantic/src/semantic_index/builder.rs#L195-L197) does not account for that.

Similarly, [this check](https://github.com/astral-sh/ruff/blob/15af4c0a34dd6d86d9aa98115ae1e1e9392c7eef/crates/ty_python_semantic/src/semantic_index.rs#L166-L177) is also too naive.

---

_Comment by @AlexWaygood on 2025-10-07 10:02_

this sounds similar to https://github.com/astral-sh/ty/issues/147

---

_Comment by @Glyphack on 2025-10-08 08:35_

I can work on this one if you are not working on it (I don't have any idea what needs to change I'll look around if you have a solution already let me know.)

---

_Comment by @sharkdp on 2025-10-08 09:34_

> I can work on this one if you are not working on it (I don't have any idea what needs to change I'll look around if you have a solution already let me know.)

That sounds great. The two source ranges I linked above are good places to start. I think we probably want to skip over eagerly-executed scopes before trying to identify the method scope (and its parent class scope). We should also take care of skipping type-parameter scopes, which I think is not done consistently at the moment: For example:
```py
class C:
    def f[T](self):
        [[... for self.x in ...] for ... in ...]
```
Here, I think we have the following scope stack when discovering the `self.x` expression (I have not checked this, might also look slightly different):
```
inner comprehension scope (eagerly executed)
outer comprehension scope (eagerly executed)
scope of function `f`
type parameter scope `[T]`
scope of class `C`
```

So if we want to properly mark (and later recognize) this as an attribute assignment to `self.x`, we should first skip all non-function eagerly-executed scopes (in this case: the two comprehensions), then verify that we are in a function scope, then skip the type parameter scope, then verify that the function is inside a class.

Maybe it makes sense to limit this to eager comprehension scopes for now(?).

This old PR is probably also interesting: https://github.com/astral-sh/ruff/pull/17012.

---

_Assigned to @Glyphack by @carljm on 2025-10-08 16:36_

---

_Closed by @carljm on 2025-11-11 22:45_

---
