```yaml
number: 19998
title: non-building draft PR for early feedback
type: pull_request
state: closed
author: oconnor663
labels:
  - ty
assignees: []
draft: true
base: main
head: jack/new_type2
created_at: 2025-08-20T04:01:52Z
updated_at: 2025-11-10T19:56:32Z
url: https://github.com/astral-sh/ruff/pull/19998
synced_at: 2026-01-12T15:56:52Z
```

# non-building draft PR for early feedback

---

_@oconnor663_

This is my trial-and-error attempt to introduce a `ClassSingletonType` enum to `crates/ty_python_semantic/src/types/class.rs`, as @carljm and @AlexWaygood and I discussed. This fixes all the immediate type errors in that file, but there are still many in other files. I wanted to check in with you guys to see if I can get some early feedback about whether I'm moving in the right direction. In particular, a lot of `ClassLiteral` methods end up getting wrapped by `ClassSingletonType` and `NewTypeClass` _and_ `ClassType`, which doesn't seem ideal.

---

_Label `ty` added by @AlexWaygood on 2025-08-20 09:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1072 on 2025-08-20 10:32_

I think this is where you need to actually create the "synthetic NewType class" -- so you'll want something like this? (Haven't checked that this actually compiles!)

```suggestion
                            [Some(Type::StringLiteral(name)), Some(supertype)] => {
                                if let Some(supertype) = supertype.to_class_type(db) {
	                                let synthesized_class = NewTypeClass::new(
	                                    db,
	                                    &name.value(db),
	                                    supertype
	                                );
	                                overload.set_return_type(Type::ClassSingleton(
	                                    ClassSingletonType::NewType(synthesized_class)
	                                ));  
                                }
                            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1294 on 2025-08-20 13:00_

The definition of the `NewType` synthesized class is the `N = NewType("N", int)` function call, so we need to return a `Definition` instance that can point back to that original `ast::Expr::Call` node in the AST.

I think you probably just want to store the `Definition` directly on the `NewTypeClass` struct, similarly to how we do for `TypeVarInstance`?

https://github.com/astral-sh/ruff/blob/276405b44eefd70e212abe1d37a11ce419ec6813/crates/ty_python_semantic/src/types.rs#L7088

You can figure out what the `Definition` for the NewType is by looking it up in the semantic index using this method:

https://github.com/astral-sh/ruff/blob/49942f656cf881619f8cab6ad75fd99a8d8d3935/crates/ty_python_semantic/src/semantic_index.rs#L417-L436

---

_@AlexWaygood reviewed on 2025-08-20 14:11_

Some answers to a couple of questions I saw you posing in the diff!

---

_@oconnor663 reviewed on 2025-08-20 15:37_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/class.rs`:1294 on 2025-08-20 15:37_

I didn't realize how melodramatic some of my todo comments are. I probably should've scrubbed the branch...

One side topic I wanted to ask about is whether we can assume that a `Definition` exists for a `NewType`. For example Pyright allows this (clearly a definition, just with multiple parts):

```py
Foo, Bar = NewType("Foo", int), NewType("Bar", float)
```

But not this:

```py
Foo, Bar = [NewType("Foo", int), NewType("Bar", float)]
```

And certainly not this:

```py
types = [NewType("Foo", int), NewType("Bar", float)]
Foo, Bar = types
```

Do we want to impose a rule like "for `NewType` to actually work, it must be at the top level of an assignment statement"?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1294 on 2025-08-20 16:12_

> Do we want to impose a rule like "for `NewType` to actually work, it must be at the top level of an assignment statement"?

It sort-of feels like a "garbage-in, garbage out" situation, but all of pyright's behaviour here makes intuitive sense to me: https://pyright-play.net/?pythonVersion=3.13&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoByApgO4AqiRAUFQIIA0UAQlALxQAUx5lHARLT6NUMAJSNuFBEX5MhUYABswAQzGiqIIgDciKxQH140jrQ4AWAEyiNVAGJgwjJipBsoAbUm8%2BDsPJFxQlIpGT4XEHklVTEAXU0dPUNjGT8La1sUgGd3LxCfMgCUMQl8kz4AZSjlNVF4gGFGABF3bITdfSNeevSbKiA. And I think the same behaviour should naturally fall out of our implementation.

I wouldn't object to having a lint that fires if you have a `NewType` call that isn't at the top-level of an assignment, though. I can see what you mean that it could have confusing results in some cases (e.g. the unpacking from a list example), and there doesn't seem much of a use case for anything except the direct `X = NewType("X", int)` way to define a `NewType`.

---

_@AlexWaygood reviewed on 2025-08-20 16:12_

---

_@AlexWaygood reviewed on 2025-08-20 16:21_

Thank you for opening this PR for early feedback!! I found it very clarifying. In particular, I chatted with @carljm and I'm afraid we think we might have led you astray, which is totally our fault :-(

In our meeting the other day, we chatted about how `NewType`, the functional namedtuple syntax, the functional enum syntax, the functional TypedDict syntax, and the builtin `type()` function are all similar: we said that they all create class objects via function calls. But... one of those is not like the others, and, unfortunately, it's specifically `NewType` that's not like the others. Calling `NewType` doesn't actually return a class object; it returns a dummy object that, when called, acts like an identity function:

```pycon
>>> from typing import NewType
>>> N = NewType("N", int)
>>> N
__main__.N
>>> type(N)
<class 'typing.NewType'>
>>> hasattr(N, "__mro__")
False
>>> isinstance(N, type)
False
```

That means that it probably honestly _isn't_ a great idea to represent the object returned by `NewType()` as a subvariant of `Type::ClassLiteral` afterall. Having it as a subvariant of `Type::ClassLiteral` would imply that all attributes available on instances of type are available on objects returned by `NewType()`, that `N` in the example above would inhabit the type `type[int]`, that `N` would be a valid object to pass as the second argument to `issubclass()` or `isinstance()`, and that `type[N]` would be a valid type annotation for a function parameter... but none of these are actually true. We could workaround all these implications by doing `match`es on the inner enum wrapped by `Type::ClassLiteral`, but it might be a better fit to represent the object returned by a call to `NewType()` as a subvariant of `Type::KnownInstance()`, and add a new variant to the `NominalInstanceInner` enum in `instance.rs` enum specifically for objects that inhabit `NewType` types.

However! I don't think all the work you've done on this big refactor was necessarily wasted. Although this might not actually be the right approach for implementing `NewType`, it _is_ probably the right approach for the function-call namedtuple syntax, the functional enum syntax or the functional TypedDict syntax (all of which actually do create class objects at runtime). You could possibly repurpose this branch into a PR adding support for the functional namedtuple syntax...? (https://github.com/astral-sh/ty/issues/1049)

---

_Comment by @oconnor663 on 2025-08-21 14:02_

> Having it as a subvariant of `Type::ClassLiteral` would imply...that `type[N]` would be a valid type annotation for a function parameter

Indeed this doesn't seem to work, but it's not clear to me why it doesn't:

```py
from typing import NewType
Foo = NewType("Foo", int)
def f(x: type[Foo]): ...
f(Foo)  # pyright: error: Argument of type "type[Foo]" cannot be assigned to parameter "x" of type "type[Foo]"
```

Do you know why Pyright doesn't allow that?

---

_Comment by @AlexWaygood on 2025-08-21 16:30_

> Indeed this doesn't seem to work, but it's not clear to me why it doesn't:
> 
> ```python
> from typing import NewType
> Foo = NewType("Foo", int)
> def f(x: type[Foo]): ...
> f(Foo)  # pyright: error: Argument of type "type[Foo]" cannot be assigned to parameter "x" of type "type[Foo]"
> ```
> 
> Do you know why Pyright doesn't allow that?

It's a somewhat confusing diagnostic from pyright in my opinion; I think it would be better for pyright to emit the diagnostic on the `f` function that tries to use the type annotation `type[Foo]` rather than allowing the annotation but then essentially considering it an "uninhabited type".

`type[Foo]` here doesn't really make sense as a concept because `type[Foo]` means "the class object `Foo` or any subclass of `Foo`". But `Foo` isn't a class object at runtime, and cannot be subclassed. Many of the properties that you would expect any inhabitant of a `type[]` type to have do not hold true for the runtime object `Foo`, as well: it doesn't have an `__mro__` attribute, a `__base__` attribute or a `__bases__` attribute (these are attributes that all class objects have, and any inhabitant of a `type[]` type must be a class object).

---

_Comment by @MichaReiser on 2025-11-10 17:44_

Should we close this now that your other PR is about to land?

---

_Closed by @oconnor663 on 2025-11-10 19:56_

---
