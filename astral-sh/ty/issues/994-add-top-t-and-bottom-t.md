```yaml
number: 994
title: "Add Top[T] and Bottom[T]"
type: issue
state: closed
author: JelleZijlstra
labels: []
assignees: []
created_at: 2025-08-15T04:53:37Z
updated_at: 2025-09-19T17:42:17Z
url: https://github.com/astral-sh/ty/issues/994
synced_at: 2026-01-12T15:54:24Z
```

# Add Top[T] and Bottom[T]

---

_@JelleZijlstra_

I'd like to propose adding two new type constructors, `ty_extensions.Top[T]` and `Bottom[T]`. `Top[T]` represents the top materialization of `T`, the smallest type that is a supertype of all materializations of `T`. `Bottom[T]` is the bottom materialization, the largest type that is a subtype of all materializations of `T`. These types would be exposed in `ty_extensions` for testing, and ty would synthesize them internally in some circumstances.

There are a few possible use cases for these types:

- `isinstance()` with generic types (#456). For example, `isinstance(x, list)` could be implemented like `TypeIs[Top[list[Any]]]`.
- A more elegant implementation of various operations on gradual types, such as assignability and equivalence (#666). `A assignable-to B` can be implemented as `Bottom[A] <: Top[B]`; `A equivalent-to B` is `Top[A] = Top[B] and Bottom[A] = Bottom[B]`. In general, the top/bottom materialization allows you to transform operations on gradual types to operations on fully static types.
- The top and bottom materializations also show up in some approaches for implementing operations on intersection and negation types, as I [discussed](https://jellezijlstra.github.io/negation-types) for negation types.
- `Top[T]` could potentially become part of the "public" type system later (i.e., something users would be expected to write in their own code). For example, it can be useful to express that a function accepts any instance of an invariant generic type, regardless of the generic parameter. (Not so much for `Bottom[T]`; I have a hard time coming up with uses for that as a public type.)

There's also some risks:

- It may hurt performance to create more complex implementations of common operations like assignability. Perhaps caching could help.
- But if we add caching, it may also hurt memory usage.
- Error messages may get worse if we're not careful, since we might end up exposing internal-only Top and Bottom types that confuse users.
- There are some theoretical issues with these types in the context of Python's type system. It may be possible to work around these issues and end up with something practically useful, but it's also possible that these issues mean that the idea doesn't work out in practice. In particular:
  - The bottom materialization doesn't fit well in a set-theoretic formulation of Python's type system, because in many cases (such as `Bottom[list[Any]]`) it's logically equivalent to Never, as there is no value that is a member of `Bottom[T]`. However, if we actually reduced these types to `Never`, assignability wouldn't work the way we'd want. So we have to treat `Bottom[list[Any]]` as an irreducible type and define how things like subtyping work on it. My intuition is that it's possible to do this in a way that ends up with a sensible set of results, but the theoretical foundation is shaky.
  - `Top[T]` for a fully static type is the same as `T`, but what is a fully static type? There's reason to say that if any attribute on a type `T` is a gradual type, then the value of that attribute on `Top[T]` is the `Top` of that gradual type. If we don't do this, then subtyping between protocols and nominal types might not behave quite correctly. But `object` contains `Any` in several of its attributes, so does that mean `Top[object]` is a distinct type that is a supertype of `object`? I am not sure yet if there's a good solution here.

Given those risks, it's somewhat speculative whether this project would actually work out, but I'm interested in working on it and seeing if I can make it work. Let me know if you think it's reasonable to try it out!

My plan would be along the following lines:

- Do some more thinking about the theory behind this, particularly the issue listed above about Any inside nominal types.
- Add new type variants for `Top` and `Bottom`, and fill in basic scaffolding for them.
- Implement the necessary operations on top and bottom types, such as:
  - Simplification (e.g., `Top[Top[T]] = Top[T]`).
  - Subtyping
  - Attribute access
  - Disjointness
- Start using Top and Bottom internally for implementing other operations
  - For assignability, run both this and the current implementation and see if they match


---

_Comment by @carljm on 2025-08-15 18:09_

This is great, would be very happy to have you looking into these improvements in ty!

We do already implement top and bottom materialization, via `ty_extensions.top_materialization` and `ty_extensions.bottom_materialization` functions. These should really be bracketed special forms as you propose, though, rather than functions (I think @sharkdp already mentioned this in post-land review of the PR that added them).

At the moment we only use materialization for overload evaluation, and our implementation is not fully correct at the moment: most notably in that we don't have any representation for the type resulting from bottom or top materializing an invariant generic like `list[Any]`.

Currently we don't have any representation for top or bottom materialization _as a type_. It's rather a type transformation that always results in a fully-static output type, represented using our existing type representations. I think this is preferable for simplicity, if we can manage to cover all cases this way? For example, we can add a representation in `Specialization` that allows specializing some typevar to "all possible specializations", meaning we would "simplify" `Top[list[Any]]` to `list[*]`, rather than leaving it as an irreducible `Top` type. Similarly, `Bottom[list[Any]]` could be represented as something like `list[_]`. The reason I think this may be preferable, if possible, is that proliferation of top-level `Type` variants quickly adds a lot of complexity, as those types need to be handled in lots of places. Handling these new special cases in specialization would be much more targeted. Ultimately I think this is a question of implementation, not theory.

> * `Top[T]` for a fully static type is the same as `T`, but what is a fully static type? There's reason to say that if any attribute on a type `T` is a gradual type, then the value of that attribute on `Top[T]` is the `Top` of that gradual type.

I think the answer here has to be that a nominal class type is a fully static type even if it has non-fully-static attributes; a nominal type is defined solely nominally, not structurally. Otherwise we get into some problematic situations:

```py
class A:
    x: int

class B(A):
    x: Any
```

This has always been accepted as a valid subclass, and I don't think we can change that. But if we say that `Top[B]` has `x: object`, then it is unsound to have `Top[B]` as a subtype of `Top[A]`, and I don't think that's tenable. The alternative would have to be that `Top[B]` materializes its `x` to "the top type that is valid for its inheritance relationships". For Top that is feasible but complex. For Bottom its impossible, we can't see all subclasses.

> * If we don't do this, then subtyping between protocols and nominal types might not behave quite correctly.

As far as I'm aware, the main consequence here is that we have to define subtyping `S <: T` as `Top[S] <: Bottom[T]` instead of as `Bottom[S] <: Bottom[T] && Top[S] <: Top[T]`, in order to preserve transitivity of subtyping between protocols and nominal types. This is too bad, but manageable.

> Given those risks, it's somewhat speculative whether this project would actually work out, but I'm interested in working on it and seeing if I can make it work. Let me know if you think it's reasonable to try it out!

Definitely! I think the initial step should be to add a proper representation of `Top[list]` and use that in narrowing cases such as `isinstance(X, list)` -- this seems low-risk and addresses a major current shortcoming of ty; it will definitely be useful regardless of whether later steps work out or not.

---

_Comment by @jelle-openai on 2025-08-15 18:50_

> we would "simplify" Top[list[Any]] to `list[*]`

I think this doesn't work because `Top[list[list[Any]]` is a different type than `list[Top[list[Any]]]`, but which of those does `list[list[*]]` mean?

---

_Comment by @carljm on 2025-08-15 18:58_

> I think this doesn't work because `Top[list[list[Any]]` is a different type than `list[Top[list[Any]]]`, but which of those does `list[list[*]]` mean?

That's a syntactic ambiguity of a possible display (which does mean we shouldn't use that display), but it also reveals the potential implementation ambiguity if we tried to represent this as an alternate value for the specialization of a particular typevar.

It still seems likely that this can be represented in a way that is specific to generic classes, rather than a fully general wrapper type, if invariant generic classes are the only case where we currently can't represent a Top type. (They are the only one that came up when we implemented the materialization transform, AFAIR.) This would still represent it in the `Specialization`, but as an attribute of the entire `Specialization`, not the substitution type for a particular typevar. Then there is no ambiguity -- `list[Top[list[Any]]` has the marker only on the inner list's Specialization, `Top[list[list[Any]]` only on the outer Specialization.

(ETA: the marker could also still be specific to a particular typevar in the specialization, if it were a marker that's in addition to a specialized type, not instead of it.)

---

_Comment by @JelleZijlstra on 2025-08-16 01:29_

We'd also need this for invariant cases that aren't generics, like:

```
class TD(TypedDict):
    x: Any
```
Where `Top[TD]` would be something like a TypedDict with key `x: *`. I guess in working on this I'll have to discover how to represent that in ty :). I definitely see the appeal of avoiding a new top-level type.

---

_Comment by @carljm on 2025-08-22 14:08_

Also relevant: https://github.com/astral-sh/ty/issues/666 (oh nm that was already mentioned in the OP here)

---

_Comment by @JelleZijlstra on 2025-08-22 22:58_

OK, since the main motivation for doing this now is going to be `isinstance(..., list)` support, I'm going to focus on the aspects of this idea that will help that use case.

I'm now thinking of the following series of operations, each of which should be an independent PR that can be merged separately:

1. Add `ty_extensions.Top[]`, replacing `ty_extensions.top_materialization()`, so that we can use it in type annotations and test its behavior that way.
2. Add a representation for `Top[list[Any]]` (and `Top[list[int & Any]]` etc.), probably as some sort of flag on the generic specialization.
3. Support operations on Top[] types that may not be supported yet. For example, given `x: Top[list[Any]]`, `x.append()` should have `Never` as its parameter type (in contravariant positions, the type parameter gets specialized to Never), while `x[0]` returns `object`.
4. Once `Top[list[Any]]` has reasonable behavior, edit the `isinstance()` code to infer it.

---

_Comment by @carljm on 2025-08-22 23:02_

Yes, that seems right to me!

---

_Comment by @JelleZijlstra on 2025-08-23 18:56_

In addition to invariant generics and TypedDicts, we might also need a special representation for the top/bottom materialization of protocols.

```python
from typing import Protocol

class P(Protocol):
    attr: object
    
class X:
    attr: int
    
def f(p: P): pass
def g(p: X): f(p)  # error, int is not consistent with object
```

This program is rejected, which implies that if we have a protocol with `attr: Any`, all the materializations to static types are incompatible (they are not mutually assignable).

But maybe we'd be able to synthesize something like this?

```python
from typing import Any, Protocol, Never

class P(Protocol):
    attr: Any

class TopP(Protocol):  # equivalent to Top[P]?
    @property
    def attr(self) -> object: ...
    @attr.setter
    def attr(self, value: Never) -> None: ...

class X:
    attr: int

def topp(p: TopP): ...
def p(p: P): ...
def g(x: X):
    topp(x)  # OK
    p(x)  # OK
```
(That is, you replace an attribute of type T with a property for which the getter returns Top[T] and the setter takes Bottom[T]. For the bottom materialization you can do the reverse.)





---

_Comment by @AlexWaygood on 2025-08-23 19:01_

Our protocol implementation is still very dumb right now and still often doesn't take account of the types of members during subtyping/assignability checks (I'm working on it). So I'd leave thinking about protocols till last; you'll get all sorts of funny results with protocols on `main` right now.

---

_Comment by @carljm on 2025-09-19 15:53_

There are further ideas in the OP here about how `Top` and `Bottom` types could be used in ty, but according to a simple reading of the issue title, this issue is now fixed -- we have those types. @JelleZijlstra do you prefer to keep this issue open as-is (in that case, we should clarify what exactly would cause us to consider it complete), or close it and open new issues for specific new target functionality?

---

_Comment by @JelleZijlstra on 2025-09-19 17:42_

Let's close this for now. I might work more in this area later but have a couple of other projects I'm working on first.

---

_Closed by @JelleZijlstra on 2025-09-19 17:42_

---
