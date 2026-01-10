```yaml
number: 336
title: "Error emitted because object is inferred as having `Element[Literal[\"testsuites\"]]` when `Element[str]` was expected"
type: issue
state: closed
author: Guitaricet
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-05-12T21:27:54Z
updated_at: 2025-09-18T00:21:13Z
url: https://github.com/astral-sh/ty/issues/336
synced_at: 2026-01-10T02:06:24Z
```

# Error emitted because object is inferred as having `Element[Literal["testsuites"]]` when `Element[str]` was expected

---

_Issue opened by @Guitaricet on 2025-05-12 21:27_

### Summary

Thank you so much for ty, it's already much better than the alternatives in both speed and ease of setup.

The error I'm seeing suggests that ty currently does not resolve Literal values into their actual types.

E.g., ty fails this code
```
import xml.etree.ElementTree as ET

testsuites = ET.Element("testsuites")
testsuite = ET.SubElement(testsuites, "testsuite")
# ^^^^^^^^^^ Expected `Element[str]`, found `Element[Literal["testsuites"]]`
```

Full output:
```
❯❯❯ ty check ty_error.py
error: lint:invalid-argument-type: Argument to this function is incorrect
 --> ty_error.py:4:27
  |
3 | testsuites = ET.Element("testsuites")
4 | testsuite = ET.SubElement(testsuites, "testsuite")
  |                           ^^^^^^^^^^ Expected `Element[str]`, found `Element[Literal["testsuites"]]`
5 | # ^^^^^^^^^^ Expected `Element[str]`, found `Element[Literal["testsuites"]]`
  |
info: Function defined here
   --> stdlib/xml/etree/ElementTree.pyi:140:5
    |
138 |     def __bool__(self) -> bool: ...
139 |
140 | def SubElement(parent: Element, tag: str, attrib: dict[str, str] = ..., **extra: str) -> Element: ...
    |     ^^^^^^^^^^ --------------- Parameter declared here
141 | def Comment(text: str | None = None) -> _CallableElement: ...
142 | def ProcessingInstruction(target: str, text: str | None = None) -> _CallableElement: ...
    |
info: `lint:invalid-argument-type` is enabled by default

Found 1 diagnostic
```

Expected behavior: ty should not fail this code.
Tested it with both `ty-0.0.0a7` and `ty-0.0.0a8` (commit `994caa6fcc2fdc7ab5e56dd1b86e3075aa404b9a`)

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-05-12 21:38_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-05-12 21:38_

---

_Renamed from "Literal strings are not resolved into strings" to "Error emitted because object is inferred as having `Element[Literal["testsuites"]]` when `Element[str]` was expected" by @AlexWaygood on 2025-05-12 22:01_

---

_Comment by @AlexWaygood on 2025-05-12 22:27_

Although `Literal["testsuites"]` _is_ assignable to `str`, ty is correct in thinking that `Element[Literal["testsuites"]]` is not assignable to `Element[str]` That's because `Element` is generic over an invariant TypeVar in typeshed's stubs (and typeshed's stubs are what ty uses as the sole source of truth for types in the standard library):

https://github.com/python/typeshed/blob/41b2f9e7b4f0494b26052552d590886ed524b4aa/stdlib/xml/etree/ElementTree.pyi#L86-L89

So the issue here is the fact that we infer the `testsuites` variable as being of type `Element[Literal["testsuites"]]` at all -- we need to avoid that, and instead infer it as some type that _is_ assignable to `Element[str]`. There are several ways in which we could accomplish this:
- Since no explicit type annotation was provided for `testsuites`, we could infer the type of the variable as being `Element[Unknown | Literal["testsuites"]]`, which would be assignable to `Element[str]`.
- We could apply a general principle that we should always upcast `Literal` types when inferring specialisations (I believe that is what both mypy and pyright are doing here); this would mean that we would infer `Element[str]` for the variable
- We could do something similar to what Rust does in many situations when inferring generic specialisations, and what mypy in particular does in lots of situations (though I don't think it's what mypy is doing here): bidirectional type inference. We would look later on in the same scope and see that an object of type `Element[str]` is required in order for the `testsuite = ET.SubElement(testsuites, "testsuite")` call to succeed without any typing errors, and use that information as part of our decision-making process when deciding what type to infer for the `testsuites` variable.

All of these options have their tradeoffs in terms of the user experience and their implementation complexities. It's a general issue that we're aware of and that we're still debating how to fix.

---

_Comment by @Guitaricet on 2025-05-13 00:42_

Thank you for a quick reply! I think all these solutions sounds reasonable. It might be easier to switch from pytype to ty if they follow similar logic, but there are probably some issues with pytype's approach that I don't know about (and it's very slow).

I think I don't fully understand the advantage of the first approach, because we explicitly provide a `"string literal"` and I don't even know how to make it not literal. So, assuming that it's a union of `Unknown|Literal` is a bit dangerous, because the type is is fact known. Especially from the standpoint of the developer -- all code is very explicit with datatypes here.

Anyways, thanks for reacting. Just wanted to provide another potential test-case.

---

_Comment by @JelleZijlstra on 2025-05-13 01:42_

I think this is also possibly a typeshed issue. `SubElement` is [declared](https://github.com/python/typeshed/blob/8b877a6993cf183f865a2c11c1f89adb6156f5c0/stdlib/xml/etree/ElementTree.pyi#L140) as taking just `Element`, which is `Element[str]` because of a TypeVar default, but I suspect it doesn't actually require a specific kind of Element.

(Side note: I feel we may have gone a bit overboard with TypeVar defaults. They can and do cause subtle usability issues. It's also sort of annoying that `Element` is invariant in its TypeVar; it can't be covariant because in theory somebody could be assigning to the `tag` field.)

Even if a typeshed fix helps in this specific case, you might run into similar issues elsewhere too. The `Unknown |` solution feels a bit dangerous because you're injecting (essentially) Any into what looks like fully typed code. Upcasting Literals in inferring generics feels a little unprincipled but might work well in practice.


---

_Comment by @dcreager on 2025-05-14 14:48_

> * We could apply a general principle that we should always upcast `Literal` types when inferring specialisations (I believe that is what both mypy and pyright are doing here); this would mean that we would infer `Element[str]` for the variable
> * We could do something similar to what Rust does in many situations when inferring generic specialisations, and what mypy in particular does in lots of situations (though I don't think it's what mypy is doing here): bidirectional type inference. We would look later on in the same scope and see that an object of type `Element[str]` is required in order for the `testsuite = ET.SubElement(testsuites, "testsuite")` call to succeed without any typing errors, and use that information as part of our decision-making process when deciding what type to infer for the `testsuites` variable.

There's also an option in between these two, which you could consider a hacky approximation of bidirectional type checking: infer the literal type when inferring a specialization of a function, and infer the instance type when inferring a specialization of a class. The rationale being that a class specialization is more likely to be "persistent" or "long-lived", and therefore more likely that the widened instance type is what is intended. But that a function specialization is only (and can only be) used for a specific call site, and therefore more useful to propagate the more specific literal type into the return type.

(And to clarify, this would be a stopgap, not a long-term solution to this issue.)

---

_Comment by @carljm on 2025-05-14 18:32_

I think the hybrid stopgap might lead to some unfortunate interactions in generic functions returning instances of generic classes?

```py
class C[T]:
    x: T

def f[T](x: T) -> C[T]:
    return C(x)

f(1)
```

Is the last line an error because the generic function thinks it returns `C[Literal[1]]` but the class instantiation inside the function wants it to be `C[int]`?

---

_Comment by @dcreager on 2025-05-14 18:50_

> Is the last line an error because the generic function thinks it returns `C[Literal[1]]` but the class instantiation inside the function wants it to be `C[int]`?

I don't know that I'd call it an error — at least not in the sense of a diagnostic being produced.  The revealed type for this example in https://github.com/astral-sh/ruff/pull/18102 is specialized with a literal, though:

```py
class C[T]:
    def __init__(self, x: T) -> None: ...

def f[T](x: T) -> C[T]:
    return C(x)

reveal_type(f(5))  # revealed: C[Literal[5]]
```

You might expect to get an `invalid-return-type` diagnostic, but that could only come from in the function body, and the literal type doesn't propagate from the call site into the function body.

Are you thinking that this might be an unsoundness?

---

_Comment by @carljm on 2025-05-14 18:55_

> The `Unknown |` solution feels a bit dangerous because you're injecting (essentially) Any into what looks like fully typed code.

A couple thoughts here:

1. The issue of "injecting Any into what looks like fully typed code" is kind of deeply baked into the typing spec for generics, in that the "default default" for any typevar is `Any`. So I'm not sure this is really all that new. You could see `Element[str]("testsuites")` as the "fully typed" version and `Element("testsuites")` as the "you didn't specify any upper bound" version, with implicit fallback, just like any other use of a not-explicitly-specialized generic type. (Although it seems like that fallback actually would be to `Element[str]` in this case, not `Element[Any]`, due to the typevar default -- which means if we inferred `Element("testsuites")` as `Element[upper-bound | lower-bound]` it would actually become `Element[str | Literal["testsuites"])` (not `Element[Unknown | Literal["testsuites"]]`), which of course simplifies to just `Element[str]`. Which is the desired behavior in this case.
2. I also think it's less dangerous than it seems, because of the preservation of the known lower bound (the `| Literal["testsuites"]` part of the type), which ensures that anytime you pull something _out_ of the generic and try to use it in a way that would be an error for a string, you still get an error. I think that overall this approach gives a very good balance of avoiding assuming unspecified intent (and emitting false positives as a result), while still avoiding false negatives.
3. I think really the most challenging part of this idea is that if we e.g. infer `list[Unknown | Literal[1]]` for `mylist = [1]`, we _have_ to be able to flow-sensitively "narrow" its type to `list[Unknown | Literal[1] | Literal["foo"]]` after `mylist.append("foo")`, or the whole approach falls apart. This might turn out to be a deal-breaker (due to implementation complexity) in practice.

---

_Comment by @dcreager on 2025-05-14 18:57_

I mentioned in https://github.com/astral-sh/ruff/pull/18102 but not here that the literal promotion also does not happen when you explicitly specialize a generic class:

```py
class C[T]:
    def __init__(self, x: T) -> None: ...


reveal_type(C(1))  # revealed: C[int]
reveal_type(C[int](1))  # revealed: C[int]
reveal_type(C[Literal[1]](1))  # revealed: C[Literal[1]]
```

If you squint this framing applies to the generic-function-returns-generic-class example, too — you've explicitly specialized the class in the return type annotation, and so my proposed literal promotion doesn't apply.

---

_Comment by @carljm on 2025-05-14 19:36_

> You might expect to get an `invalid-return-type` diagnostic, but that could only come from in the function body, and the literal type doesn't propagate from the call site into the function body.
> 
> Are you thinking that this might be an unsoundness?

No, you're right, I wasn't thinking clearly; this is a non-issue.

---

_Comment by @mikeshardmind on 2025-05-16 15:32_

I've seen this now with `Literal[False]` being inferred and erroring, where a `bool` inference would work.

I think the general principle here should be not to infer value refinements (Literals in python's type system) for non-final variables, as the value based refinement would preclude changing the value anyway.

---

_Comment by @carljm on 2025-05-16 17:35_

Is `None` a "value-based refinement" type?

---

_Comment by @jelle-openai on 2025-05-16 17:42_

> I think the general principle here should be not to infer value refinements (Literals in python's type system) for non-final variables, as the value based refinement would preclude changing the value anyway.

In the example in this thread, there are many different `Element[Literal["testsuites"]]` that could exist, so the inference of a Literal type argument doesn't preclude the user from changing the value.

---

_Comment by @mikeshardmind on 2025-05-16 18:33_

> Is None a "value-based refinement" type?

No, it's an unfortunate issue where `None` is inconsistent with the rest of the type system. As a type expression, it actually means `types.NoneType`, whereas `1` not only doesn't mean `int`, but isn't valid as a type expression.

Literals (as in `typing.Literal[...]` as a type expression) are a a refinement of a type to a specific value.


The problem with refining by value in inference is trivially obvious, one assignment doesn't mean only that exact value is allowed, but the attempt to find "the most precise type" and treating refinements as more precise for this applied consistently would prevent the below:

```py
x = False
x = True
```

What's actually being found is a more precise known value.

https://play.ty.dev/0d1f4c28-3e42-474f-914d-3500280e8b70

Here, ty actually overwrites this inferring Literal[False], then Literal[True]

An example of how this goes wrong *very* fast is with contextvars: 

```py
from contextvars import ContextVar

c = ContextVar("c", default=False)
c.set(True)  
```

> Argument to bound method `set` is incorrect: Expected `Literal[False]`, found `Literal[True]` (invalid-argument-type)

https://play.ty.dev/93c954fd-b4a4-4af5-8497-61812d1686cf




---

_Comment by @mikeshardmind on 2025-05-16 19:11_

There are some other kinds of inconsistencies with ty's current inference of Literals: <https://play.ty.dev/f3f6ce69-c9bf-4018-bafa-94d7c7bf1b09> 

Happy to discuss options either here, or less formally over on discord, but Literals tend to be particularly bad for inference.
For clarity on those not familiar with the term "Refinement type": <https://en.wikipedia.org/wiki/Refinement_type> is correct enough, and the reasoning of not narrowing to them in inference because that's narrowing by value, not by type here would hold for generalized refinement types, not just python's current only form of them with Literals.



---

_Comment by @dcreager on 2025-05-19 20:21_

@mikeshardmind We definitely agree that inferring a literal type for a class specialization isn't right. Your `ContextVar` example is another instance of exactly what we're trying to resolve with this discussion. We just merged https://github.com/astral-sh/ruff/pull/18102, which implements the stopgap option I described upthread; with that merged the `invalid-argument-type` diagnostic goes away. [[playground](https://play.ty.dev/93c954fd-b4a4-4af5-8497-61812d1686cf)]

---

> The problem with refining by value in inference is trivially obvious, one assignment doesn't mean only that exact value is allowed, but the attempt to find "the most precise type" and treating refinements as more precise for this applied consistently would prevent the below:
> 
> ```py
> x = False
> x = True
> ```

These assignment examples are a separate discussion. This example, as written, is allowed by ty, and highlights what is possibly more of a bug in how our playground renders things. We track declared types and inferred types separately. The type hints that are shown in the playground:

![Image](https://github.com/user-attachments/assets/9cbbc680-9a1f-47ce-a018-b13b9c294303)

are showing the inferred types for those expressions, not their declared types. There are no annotations, and so we're using `Unknown` as the implicit declared type for `x`, and so both assignments are accepted.

For the inferred type we purposefully infer the more precise literal value type. That's what shown in the inlay type hint. It's also what's used when passing `x` in as an argument in a call, as seen in e.g. `reveal_type` diagnostics [[playground](https://play.ty.dev/6efb64d9-d58a-4c64-85e7-788298d8c0eb)]:

```py
from typing_extensions import reveal_type

x = False
reveal_type(x)  # revealed: Literal[False]
x = True
reveal_type(x)  # revealed: Literal[True]
```

I say that you could consider this a rendering bug in the playground, because if you "accept" the first inlay hint, putting it into the code as an actual annotation:

```py
x: Literal[False] = False
x = True
```

then the second assignment does fail as you say [[playground](https://play.ty.dev/776e5573-5446-48dd-beb2-9f267aa2113c)], since `True` is not assignable to the (now explicit) declared type of `x`. So it's arguably confusing that you have to notice the subtly different coloring of the inlay hint (which isn't actually in the source file) vs the declared type annotation (which is), and that there is different behavior for the two.

![Image](https://github.com/user-attachments/assets/98a995f7-87e3-4cf3-98a6-58dcfe69c3a8)

Adding another wrinkle, if you accept _both_ inlay hints:

```py
x: Literal[False] = False
x: Literal[True] = True
```

then both assignments are accepted again, since ty allows redefinitions of a symbol at a new type if you explicitly annotate the later definition. [[playground](https://play.ty.dev/fe3a737d-544b-4638-a0df-1b0344f36da9)]

---

_Comment by @ibraheemdev on 2025-09-17 23:06_

A similar issue came up in https://github.com/astral-sh/ruff/pull/20360. We now unconditionally promote any literals within the elements of a list/set literal, partly for performance reasons with large list literals. However, this means that we fail to typecheck against literal annotations, e.g.,
```py
# error: Object of type `list[int]` is not assignable to `list[Literal[1, 2, 3]]`
x: list[Literal[1, 2, 3]] = [1, 2, 3]

# error: Object of type `list[str]` is not assignable to `list[LiteralString]`
x: list[LiteralString] = ["a", "b", "c"]
```

Which is the same issue we currently run into with generic classes:
```py
# error: Object of type `tuple[X[int]]` is not assignable to `tuple[X[Literal[1]]]`
x: tuple[X[Literal[1]]] = (X(1), )
```

My reading of this issue is that we promote *should* eagerly promote literals, unless there is an explicit type annotation that would require otherwise? That seems to align with pyright's behavior as well.

@carljm pointed out that this could get tricky with nested types, or even unions:
```py
x: list[int | Literal["a"]] = [1, 2, 3, "a", 4, 5, 6]
reveal_type(x)
```

And a lot of this will also be influenced by how far we want to stretch the type context. For example, pyright errors on the following ([taken from `streamlit`](https://github.com/streamlit/streamlit/blob/9d0e6bdcd383a52a75a12224c84f5e71d886e4a6/lib/streamlit/config.py#L313)):
```py
def x(a: Literal["a"]): ...

x(["a"])
```

---

_Comment by @carljm on 2025-09-18 00:21_

I think this issue should be closed. We haven't errored on the code sample in the OP for a long time now -- the issue described in the OP is fixed. There are a constellation of related issues discussed here, but it will be more useful to open individual targeted actionable issues for specific problems with ty's actual behavior. I've opened #1198 to track the specific issue described in @ibraheemdev 's latest comment.

> For example, pyright errors on the following ([taken from `streamlit`](https://github.com/streamlit/streamlit/blob/9d0e6bdcd383a52a75a12224c84f5e71d886e4a6/lib/streamlit/config.py#L313)):
> 
> def x(a: Literal["a"]): ...
> 
> x(["a"])

I think this snippet is missing a `list[...]`? It should have `a: list[Literal["a"]]` as the function parameter. As written, an error is the only correct result.

With that change, both pyright and mypy handle this example fine, and we should as well.

I'm not sure this example as written demonstrates "stretching the type context" either? I would consider a function parameter annotation to be "immediate" type context, similar to an annotated assignment -- the "call argument" syntactic position is directly associated with the annotated function parameter. We definitely will need type context to apply in this scenario.

The more questionable cases are where the inferred expression is separated from the type context, e.g. if we had `foo = ["a"]` and then later `x(foo)` instead.


---

_Closed by @carljm on 2025-09-18 00:21_

---
