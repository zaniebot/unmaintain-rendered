```yaml
number: 94
title: "ty: Fails to accept special forms as types"
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - help wanted
  - type properties
assignees: []
created_at: 2025-05-07T03:27:08Z
updated_at: 2025-07-24T18:00:20Z
url: https://github.com/astral-sh/ty/issues/94
synced_at: 2026-01-12T15:54:22Z
```

# ty: Fails to accept special forms as types

---

_@JelleZijlstra_

### Summary

```python
from collections.abc import Callable
from typing import Protocol

def gimme_ty(t: type):
    pass

gimme_ty(Callable)
gimme_ty(Protocol)
```

```
    Argument to this function is incorrect: Expected `type`, found `typing.Callable` (lint:invalid-argument-type) [Ln 7, Col 10]
    Argument to this function is incorrect: Expected `type`, found `typing.Protocol` (lint:invalid-argument-type) [Ln 8, Col 10]
```

https://play.ty.dev/3aebb756-4e06-414b-9445-b53a4b1b9f5f

Both of these are in fact types (instances of `builtins.type`) at runtime.

### Version

_No response_

---

_Label `bug` added by @carljm on 2025-05-07 03:43_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-07 10:10_

---

_Comment by @my1e5 on 2025-05-07 13:23_

I think this is the same error I'm seeing with `dataclasses` and `fields`?

https://play.ty.dev/331a08e9-3adf-41f4-a4a5-483a964a1b64

```py
from dataclasses import dataclass, fields


@dataclass
class Foo:
    x: int
    y: float
    z: str


for field in fields(Foo):
    print(field.name, field.type)
```

```
$ uvx ty check foo.py 
error: lint:invalid-argument-type: Argument to this function is incorrect
  --> foo.py:11:21
   |
11 | for field in fields(Foo):
   |                     ^^^ Expected `DataclassInstance`, found `Literal[Foo]`
12 |     print(field.name, field.type)
   |
info: Function defined here
   --> stdlib\dataclasses.pyi:220:5
    |
218 |     ) -> Any: ...
219 |
220 | def fields(class_or_instance: DataclassInstance | type[DataclassInstance]) -> tuple[Field[Any], ...]: ...
    |     ^^^^^^ -------------------------------------------------------------- Parameter declared here
221 |
222 | # HACK: `obj: Never` typing matches if object argument is using `Any` type.
    |
```



---

_Comment by @JelleZijlstra on 2025-05-07 13:25_

That seems like a different issue to me.

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:17_

---

_Label `subtyping/assignability` added by @AlexWaygood on 2025-05-10 18:04_

---

_Label `help wanted` added by @carljm on 2025-07-23 22:49_

---

_Unassigned @AlexWaygood by @carljm on 2025-07-23 22:49_

---

_Comment by @carljm on 2025-07-23 22:50_

I think this should be a pretty easy fix? Just a new (or fixed) match arm in `has_relation_to`. @AlexWaygood you had it assigned to you for a while, did you try to fix it and find it unexpectedly difficult?

---

_Comment by @AlexWaygood on 2025-07-24 07:24_

No I never got to it

---

_Comment by @InSyncWithFoo on 2025-07-24 11:55_

Isn't it the case that only concrete classes are assignable to `type`?

> The value corresponding to `type[C]` must be an actual class object that's a subtype of `C`, not a [special form](https://typing.python.org/en/latest/spec/glossary.html#term-special-form) or other kind of type. In other words, in the above example calling e.g. `new_user(BasicUser | ProUser)` is rejected by the type checker (in addition to failing at runtime because you can't instantiate a union).
> [...]
> However, the actual argument passed in at runtime must still be a concrete class object [...]
>
> &mdash; [<i>Special types in annotations</i> &sect; `type[]`](https://typing.python.org/en/latest/spec/special-types.html#type)

---

_Comment by @carljm on 2025-07-24 16:31_

I think the OP idea is that at runtime `Protocol` is in fact a concrete class, that's how it is implemented in `typing.py`.

But I just looked and this is not true of `Callable` -- `Callable` at runtime is not a type, it's an instance of `_CallableType` class. So I'm not sure what the rationale would be for allowing `Callable` to be assignable to `type`.

And looking at this more closely, I'm also unsure whether it makes sense for a type checker to model any of this. Typeshed just says that both `Protocol` and `Callable` are instances of `_SpecialForm` (which is of course a lie, but typeshed tells a lot of lies in `typing.pyi`). So I think in order to implement this, we'd be hardcoding knowledge about the runtime implementation of specific special forms in `typing.py`, that typeshed intentionally elides.

For reference, pyright allows both of these, and mypy errors on both.

I'm inclined to close this and say mypy's behavior, and our current behavior, are correct according to typeshed, and if a different behavior is desired, then typeshed should be changed.

@JelleZijlstra am I missing some rationale for why you think this should be allowed?

(Going ahead and closing, but happy to reopen given good rationale that I missed.)

---

_Closed by @carljm on 2025-07-24 16:31_

---

_Comment by @jelle-openai on 2025-07-24 16:42_

I think I opened this because I tried running ty on [pycroscope](https://github.com/JelleZijlstra/pycroscope) and got some errors when Callable was passed to a function annotating as accepting `type`. At runtime `collections.abc.Callable` is an instance of `type` but `typing.Callable` isn't. I feel ideally type checkers should reflect that behavior, so that `isinstance(..., type)` matches what's assignable to `type`, but I appreciate that's difficult to do if you're working purely from the typeshed stubs, and the issue isn't important enough to do anything fancy like special-casing specific special forms.

---

_Comment by @erictraut on 2025-07-24 16:52_

> For reference, pyright allows both of these, and mypy errors on both.

I think both [pyright](https://pyright-play.net/?code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSgGEDFjkBYAKFEilQE8L0sAczKVqtAArhUYOPHby2AEyTAog8hCQB9XgApUALm58kASgPsoVqBQI4cC9uoiadPXQyZFkppxu16kmDSsr5sQA) and mypy reject this. That's arguably correct because these special forms, even if they happen to be implemented as class objects, cannot be instantiated like normal classes. Neither of them are callable.

Pyright does [accept this](https://pyright-play.net/?reportMissingModuleSource=false&code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSgGEDFjkBYAKFEilQE8L0sAczKVqtAArhUYOPHadovfkID6SAB6okuTLhFUaUACp8kAMWoR21tgBMkwKIPIQkK3gApUALmOmLIBAAlN7sUOFQFAQ4ODbszhCu7jweDExEyEHxLm6ekmDSsllsQA) if you use `TypeForm` instead of `type`.

---

_Comment by @carljm on 2025-07-24 17:57_

> I think both [pyright](https://pyright-play.net/?code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSgGEDFjkBYAKFEilQE8L0sAczKVqtAArhUYOPHby2AEyTAog8hCQB9XgApUALm58kASgPsoVqBQI4cC9uoiadPXQyZFkppxu16kmDSsr5sQA) and mypy reject this.

Yes, sorry, I see that now -- I think I was fooled by the pyright playground being slow to respond when I initially pasted in the example, so it just had an out-of-date "everything looks good" message.

---

_Comment by @carljm on 2025-07-24 18:00_

> I feel ideally type checkers should reflect that behavior, so that `isinstance(..., type)` matches what's assignable to `type`

I don't disagree, but it does seem like if we care about this, then we ought to improve typeshed. (But I realize that's probably difficult to do without disruption to existing type checkers.)

> cannot be instantiated like normal classes. Neither of them are callable.

Whether something typed as `type[...]` is callable, and with what signature, is a huge soundness hole anyway, so I'm not sure I see this as a strong argument against assignability to `type` -- consistency with `isinstance(..., type)` seems like a stronger argument to me (in principle). Ideally IMO `type[...]` wouldn't imply callability at all.

---
