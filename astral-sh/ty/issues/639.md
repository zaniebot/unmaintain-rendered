```yaml
number: 639
title: "`tuple[Any]` should be assignable to `Never`"
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-06-12T05:13:01Z
updated_at: 2025-11-18T16:10:31Z
url: https://github.com/astral-sh/ty/issues/639
synced_at: 2026-01-10T01:58:59Z
```

# `tuple[Any]` should be assignable to `Never`

---

_Issue opened by @dhruvmanila on 2025-06-12 05:13_

### Summary

`tuple[Any]` should be assignable to `Never` because `Never` is one of the materialization of `tuple[Any]` and it is a subtype of `Never` so as per the [consistent subtyping relation](https://typing.python.org/en/latest/spec/concepts.html#the-assignable-to-or-consistent-subtyping-relation) it should be assignable.

Playground: https://play.ty.dev/48729f64-0a5a-4f06-82e4-39aa671872d8

```py
from typing import Any, Never
from ty_extensions import static_assert, is_assignable_to

# This is false but should be true
static_assert(is_assignable_to(tuple[Any], Never))
```

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-06-12 05:13_

---

_Label `type properties` added by @dhruvmanila on 2025-06-12 05:13_

---

_Comment by @erictraut on 2025-06-12 05:46_

Hmm, this doesn't sound right to me. `Never` isn't a materialization for `tuple[Any]`. And while `Never` is a consistent subtype of `tuple[Any]`, the converse isn't true. 

Code sample in [pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcANFAHICmAbpSAFD0AmlwUwArigMYAUAHgC5CxMnGEwOCADaUA2kTgBdAJSD6UTVACGwqrRBQAvFH6aAxFADyAaQ1aARnpp1jUOBagBREOAZaobgkpWTl9OiU3DyhLW3ogA)

```python
from typing import Any, Never

def func(x: Any, y: tuple[Any]):
    a: Never = x  # OK
    b: Never = y  # Error
    c: tuple[Never] = y  # OK
```

Perhaps you meant to say that `tuple[Never]` is a materialization for `tuple[Any]`. That statement is true, and for that reason, `tuple[Never]` is assignable to `tuple[Any]` and vice versa. It looks like ty currently [gets this wrong](https://play.ty.dev/7c4d3d6c-ebeb-4d85-ab21-f17c174fe206).




---

_Comment by @dhruvmanila on 2025-06-12 06:23_

> `Never` isn't a materialization for `tuple[Any]`.

Isn't `tuple[Never]` same as `Never` ? i.e., a materialization of `tuple[Any]` is `tuple[Never]` which gets simplified to `Never` in ty because `tuple[Never]` is an empty set.

---

_Comment by @erictraut on 2025-06-12 14:04_

> Isn't `tuple[Never]` same as `Never`?

Just because a type includes `Never` as a type argument doesn't mean it's equivalent to `Never`. By the same logic, would you consider `list[Never]` or `UserDefined[Never]` equivalent to `Never`, or are you arguing that `tuple` is somehow special?

Here's a type that includes `Never` that's definitely not equivalent to `Never`: `Callable[[], Never]`. This represents a callable type that doesn't return control back to the caller.

---

_Comment by @sharkdp on 2025-06-12 14:13_

> are you arguing that `tuple` is somehow special?

tuple is special because it's a product type. `tuple[Never, int]` is equal to `Never` because it corresponds to the Cartesian product `Never × int`. The product with the empty set is empty.

> Just because a type includes `Never` as a type argument doesn't mean it's equivalent to `Never`. By the same logic, would you consider `list[Never]`

No, `list[Never]` is not equal to `Never`. It is a type with a single inhabitant, the empty list.

For user-defined types... it depends. A generic class like the following is structurally equivalent to a tuple/product of `T` and `int`, so `C[Never]` should also be equivalent to `Never`. There is no way to construct an object of type `C[Never]`, because there is no way to generate a value for the `x` member:
```py
class C[T]:
    x: T
    y: int
```
On the other hand, for the following example, `D[Never]` is not equal to `Never`. `D(x=None)` is a valid inhabitant of `D[Never]`:
```py
class D[T]:
    x: T | None
```

I'm not 100% if that reasoning applies to class types in Python, but for tuple types, it seems correct to consider `tuple[Never]` to be equivalent to `Never`. There is no way to construct such a tuple.

---

_Comment by @erictraut on 2025-06-12 14:28_

> tuple is special because it's a product type

I'm not familiar with the term "product type", but I think we need to be careful here.

If we (the Python typing community) collectively want to treat `tuple[Never]` (or more generally, certain classes of types that are parameterized by `Never`) as equivalent to `Never`, I think that should be documented in the typing spec. Maybe this will be done as part of the work to define intersections? If there isn't some special rule in the spec that says `tuple[Never]` is equivalent to (or even assignable to) `Never`, then I don't think a type checker should make that assumption. We've worked hard to clarify the rules for type assignability in the spec because it's important for all type checkers to be consistent here.

I'll note that `tuple` is documented to be special in a few other ways [in the spec](https://typing.python.org/en/latest/spec/tuples.html#type-compatibility-rules), but `Never` is not mentioned in this section other than "A zero-length tuple (`tuple[()]`) is a subtype of `Sequence[Never]`."

---

_Comment by @jelle-openai on 2025-06-12 14:32_

I do think it's correct to say that `tuple[Never]` is equivalent to `Never`. `tuple[Never]` means a single-element tuple, where the single element cannot exist. There is no value that is a member of this type, so the type is equivalent to Never, which is the empty type. This follows naturally from the definitions we adopted, so I don't think it needs special discussion.

---

_Comment by @carljm on 2025-06-12 14:35_

Yes, I think the equivalence of `tuple[Never]` to `Never` is an unavoidable conclusion from the core semantics of Python types, which are already in the spec. I wouldn't be opposed to explicitly documenting it also, but I don't consider that a blocker for ty implementing it (we already do). There are many such edge cases which can be reasoned from the core semantics, and I'm not sure it's even a feasible goal to explicitly mention every one in the spec.

---

_Comment by @erictraut on 2025-06-12 14:52_

We've invested heavily over the past few years to clarify the rules for assignability in the spec. What I hear you arguing here is that type checkers can selectively deviate from the standard assignability rules based on a deeper understanding of the semantics of particular classes. I worry about where that will lead. I think it means that we're going to see different assignability rules adopted by each type checker. That sounds counter to the goals we've been pursuing for standardization. How do we prevent this if we don't spell this out in the spec?

---

_Comment by @carljm on 2025-06-12 18:52_

I don't think that considering `tuple[Never]` equivalent to `Never` is a deviation from anything that is currently specified or standardized. It's a logical conclusion from the (specified) core semantics of Python types, in an area that isn't otherwise currently specified more directly. (There is no explicit specification anywhere in the spec of what types are equivalent to what other types.)

I think it's useful to continue to add explicit detail to the spec in areas where we observe divergence between type checkers that causes user problems. I'm not convinced that equivalence of `tuple[Never]` to `Never` is a priority for specification, relative to other things that cause more user problems, but I'm not opposed to adding it explicitly to the spec.

---

_Comment by @jelle-openai on 2025-06-12 19:30_

Thinking about this more, I do think this line of thinking leads to problems.

- `tuple[Any]` can materialize to `tuple[Never]`, which is equivalent to `Never`.
- `tuple[int, Any]` can materialize to `tuple[int, Never]`, which is also equivalent to `Never`.
- Two gradual types are consistent with each other if they can materialize to the same type.
- Ergo, all fixed-length tuple types that contain Any are consistent with each other.

But that seems undesirable; `tuple[int, Any]` and `tuple[Any]` clearly shouldn't be assignable to each other.

How do we fix this? Maybe `tuple[Never]` shouldn't be a legal materialization of `tuple[Any]`. `tuple[Any]` is a type that describes a value that is definitely a tuple with one element, and `tuple[Never]` doesn't.

---

_Comment by @carljm on 2025-06-12 19:39_

That's an excellent point. I'll need to think a bit about this.

---

_Comment by @dcreager on 2025-06-12 20:02_

> Two gradual types are consistent with each other if they can materialize to the same type

`tuple[Never]` and `tuple[int, Never]` cannot materialize to _any_ type. So there is no type that they both materialize to.

---

_Comment by @dcreager on 2025-06-12 20:02_

No scratch that, I'm wrong

---

_Comment by @mikeshardmind on 2025-06-12 23:34_

Something to be cautious of here is not making this too broad.

while `tuple[Never]` can never have a runtime value, `tuple[Never, ...]` can for 0-length (empty, and therefore no value of type Never) tuples

---

_Comment by @mikeshardmind on 2025-06-12 23:37_

> How do we fix this? Maybe tuple[Never] shouldn't be a legal materialization of tuple[Any]. tuple[Any] is a type that describes a value that is definitely a tuple with one element, and tuple[Never] doesn't.

We can fix this by fixing Never. If we take the stance that Never *isn't* a materialization, but the absence of a materialization, this paradoxical issue evaporates.

---

_Comment by @jelle-openai on 2025-06-13 00:00_

Do you mean that Any should never be able to materialize to Never? That seems wrong to me because it's perfectly fine for e.g. `Callable[..., Any]` to materialize to `Callable[..., Never]`.

A formulation I've been thinking of is that if a gradual type is not Any, it cannot materialize to Never (or to any type equivalent to Never). That means that `Callable[..., Any]` can still materialize to `Callable[..., Never]` (which is an inhabited type), but `tuple[Any]` cannot materialize to `tuple[Never]`.

---

_Comment by @erictraut on 2025-06-13 00:31_

I'm still not convinced that `tuple[Never]` should be assignable to (or considered equivalent to) `Never`. If I understand correctly, the argument is that the set of values described by `tuple[Never]` is empty, so it is equivalent to `Never`. But is that really true? Consider the following.

```python 
class Foo(tuple[Never]): pass

x: tuple[Never] = Foo()
```

This class might not be very useful, but I think it's a legal subclass of `tuple[Never]`, and I'm able to construct an instance of it. 

If we can agree that `tuple[Never]` is not assignable to `Never`, then I don't see a need to complicate the definitions of `Never` or "materialization".

---

_Comment by @JelleZijlstra on 2025-06-13 01:01_

Subclasses of fixed-length tuple are a good point but I think the way pyright (and mypy) currently handles those is unsound.

For example, given the program:

```python
from typing import Never

class TI(tuple[int]): pass

def f(x: tuple[int] | tuple[int, str]):
    if len(x) == 0:
        reveal_type(x)
    elif len(x) == 1:
        reveal_type(x)
    else:
        reveal_type(x)

f(TI())
```

Pyright thinks the len-0 branch is unreachable, but in fact at runtime `Foo()` creates a length-zero tuple. Pyright and mypy also allow the call `TI((1, 2))`, which creates a tuple of length 2.

For an instance of such a subclass to exist, it must be a tuple containing the elements specified in the bases. That means that no instance of your `Foo` class can exist.

---

_Comment by @erictraut on 2025-06-13 02:52_

This highlights the danger of trying to justify special-case typing rules based on the way you think that the type parameters are interpreted for that class.

Here's a class that arguably satisfies the interface contract of the `tuple` class and is a legitimate subclass of `tuple[Never]`.

```python
class TI(tuple[Never]):
    def __init__(self) -> None: ...

    def __len__(self) -> Literal[1]:
        return 1

    def __getitem__(self, key: SupportsIndex | slice, /) -> Never:
        raise IndexError()
```

When it comes to assignability, I think the rules should be as simple and consistent as possible, and they should be applied mechanistically without regard for knowledge of the internal implementation details of the class.

If there's a compelling justification for deviating from these rules, then we could consider a special case, but I maintain this should be done only after public discussion and scrutiny, and it should be standardized in the typing spec. This is why I contributed the [Type Compatibility Rules](https://typing.python.org/en/latest/spec/tuples.html#type-compatibility-rules) section of the tuples chapter, which defines a few special cases that apply to tuples. Those special cases are both useful and carefully considered, and they received due public scrutiny before being accepted into the spec.

When it comes to asserting equivalence for `tuple[Never]` and `Never`, I haven't heard any argument for why we'd want to grant such a special case — especially given that it creates paradoxes and other complications.

The notion of "assignability" is foundational to the type system, and I though we had come to a clear and unambiguous definition of assignability — thanks in large part to Carl's contributions to the typing spec. My confidence has been shaken by this thread. Apparently, the foundation is weaker than I had thought. It is being argued that the definition of assignability is malleable based on how each type checker interprets the semantics and implementation details of certain classes.

---

_Comment by @mikeshardmind on 2025-06-13 04:05_

> Here's a class that arguably satisfies the interface contract of the tuple class and is a legitimate subclass of tuple[Never].

```py
class TI(tuple[Never]):
    def __init__(self) -> None: ...

    def __len__(self) -> Literal[1]:
        return 1

    def __getitem__(self, key: SupportsIndex | slice, /) -> Never:
        raise IndexError()
```

While it does under the current rules, there's a strong argument from theory that it does not satisfy the interface contract, as `__getitem__`'s function domain is uniquely different for `tuple[Never]` than for Any tuple specialized with a type for which a value can exist.

I'm willing to say that `tuple[Never]` *doesn't have* a valid materialization, and that somewhat by definition, for a value to materialize, it has to be possible for a value of that type to exist at runtime, the constant possibility of an error is a lack of a value, not an always compatible value.

I believe this is just a gap in our definitions currently.

---

_Comment by @carljm on 2025-06-13 23:30_

Lots to comment on here! Let me start with the type theory, _if_ (and I am not assuming this!) we were to set aside the question of user subclasses of tuple and assume we can treat tuple as a simple product type. I asked Guillaume Duboc (@gldubc, though not sure if I can tag here), who works (along with Giuseppe Castagna, a leading researcher in the area of set-theoretic gradual types) on the Elixir type system, how they handle this Never issue. Their answer is that they do consider their equivalent of `tuple[Never]` to be equivalent to `Never`, for the reason discussed here (both types are empty). But they also use an additional restriction on assignability (beyond just "consistent subtyping"). Any type which can materialize to `Never` is a consistent subtype of any other type which can materialize to `Never`, per the definition of consistent subtyping. But assignability is meant to establish that an inhabitant of one type can substitute for an inhabitant of another type -- this is not meaningful in the absence of inhabitants. So for A to be assignable to B, they require both that A is a consistent subtype of B, and that, if the bottom or lower bound materialization of A is `Never`, that the upper bound or top materializations of A and B be non-disjoint. Another way to describe this is that there must be some object that can inhabit some possible materialization of both A and B, in order for A to be assignable to B.

I think this approach makes sense, and I think we should update to the core concepts document in our typing spec to describe this additional restriction. I'm happy to provide that PR, if we are in agreement that it makes sense. What it would ensure, in this case, is that _even if_ we were to consider both `tuple[Never]` and `tuple[Never, int]` as equivalent to `Never`, that would still not imply that `tuple[Any]` or `tuple[Any, int]` are assignable to `Never`, or to each other.

For me, that's sufficient to conclude that regardless of how we settle the question of `tuple[Never]` vs `Never`, we should not implement what is described in this issue (`tuple[Any]` being assignable to `Never`).

That aside, I also find @erictraut's argument convincing, that an instance can exist of a user subclass of tuple which is a valid subtype of `tuple[Never]`, which means (because of user subclasses) that `tuple[Never]` is not necessarily an empty type. This would imply that in ty we should stop simplifying heterogenous tuple types containing `Never`, to `Never`.

User subclasses of tuple are tricky; because heterogenous tuple types are inherently quite special-cased (they can't be described by typeshed), it's easy to fall into the trap of forgetting user subclasses. Both ty and pyright are currently willing to assume, for instance, that the type `tuple[()]` is always falsy, which is also not necessarily true if you consider user subclasses. (I think practically this is useful, and we do the same -- we've discussed making it an error to define `__bool__` on a user subclass of `tuple`, in order to make this assumption more sound.)

@erictraut, I guess the last thing to address here is your comments on the spec and special-casing. I reject the description of ty's treatment of `tuple[Never]` and `Never` as equivalent as "special-casing" based on "implementation details", as well as the assertion that it somehow weakens the definition of assignability in the spec. I think it was a reasonable and useful effort to reason directly _from_ that definition of assignability and apply it. And I consider the outcome of that attempt to have been quite positive for ty and for Python typing. We've uncovered a theoretical gap in the definition of assignability currently given in the spec, which can now be fixed, and we've clarified our understanding of the semantics of tuple types. In the future, I intend for ty to continue to reason from core semantics and try new things that may not be done by current type checkers. Where we are right, this can lead to future more public discussion and amendment of the spec; where we are wrong, it can also lead to discussion and changes to our behavior. I don't think that it is a service to the Python typing community for new type checkers to never attempt to implement anything new or different until after it has been codified in the spec -- this places too much of a damper on improving and increasing our understanding of the Python type system.

---

_Comment by @JelleZijlstra on 2025-06-14 00:29_

> Any type which can materialize to Never is a consistent subtype of any other type which can materialize to Never, per the definition of consistent subtyping. But assignability is meant to establish that an inhabitant of one type can substitute for an inhabitant of another type -- this is not meaningful in the absence of inhabitants. So for A to be assignable to B, they require both that A is a consistent subtype of B, and that, if the bottom or lower bound materialization of A is Never, that the upper bound or top materializations of A and B be non-disjoint. Another way to describe this is that there must be some object that can inhabit some possible materialization of both A and B, in order for A to be assignable to B.

Does this mean that Never is not assignable to itself? That would break `assert_never()`.

I'm still unconvinced that instances of a subclass of `tuple[Never]` should be allowed to exist. If they do exist, then this program has a type error, but pyright allows it:

```python
from typing import assert_never, Never

class Foo(tuple[Never]): pass

def f(x: tuple[int | str]):
    match x:
        case (int(),):
            pass
        case (str(),):
            pass
        case _:
            assert_never(x)

f(Foo())
```

Pretending to be a one-element tuple by overriding `__len__` etc. doesn't fool C-implemented functions that look at the underlying tuple object directly:

```
>>> class X(tuple[str]):
...     def __getitem__(self, i):
...         return "x"
...         
>>> t = X(("y",))
>>> "yz".startswith(t)
True
```

User-defined subclasses of heterogeneous tuple do have some practical use; there are a few dozen in typeshed, mostly for "structseq" C classes. I think type checkers should disallow such subclasses if they have Never fields, and otherwise enforce that the object is constructed with the correct number of fields.

Regardless of the issues with user-defined tuple subclasses, the problem with Never that we identified in this thread exists elsewhere too. For example, the spec is explicit that TypedDicts must be instances of `dict` itself, not a subclass (https://typing.python.org/en/latest/spec/typeddict.html#class-based-syntax under "Methods are not allowed"). That means that a TypedDict with an Any item can materialize that item to Never, making the type itself equivalent to Never. Someone above mentioned user-defined classes with Any attributes too, though there perhaps there's wiggle room with descriptors.

---

_Comment by @mikeshardmind on 2025-06-14 01:46_

I think as a practical matter, we need to clear up definitions involving Never beyond what is explored in this thread, eventually, but the additional restriction Elixir uses is compatible with the changes I personally see as beneficial, and it improves a few real cases including this one.

I'm also willing to go further and say certain behaviors in user-subclasses of builtin types should be considered an error due to python's execution model rather than just pure theory to avoid putting the two in direct conflict with each other. 


---

_Comment by @erictraut on 2025-06-14 02:49_

> Another way to describe this is that there must be some object that can inhabit some possible materialization of both A and B, in order for A to be assignable to B.

That makes sense. I'm in favor of adding this clarification to the typing spec. It might make the concept harder to grok for first-time readers of the spec, but that's not a good reason to avoid it. Plus, you have a knack for describing complex concepts in simple terms.

What does that additional restriction imply for the concept of "equivalency"? I've assumed that equivalence is transitive — that is "A == B" and "B == C" implies "A == C". Furthermore, I've assumed that equivalence implies assignability — if "A == B", then "A := B" and "B := A". If those assumptions are incorrect, then I think the concept of "equivalence" loses most of its utility. Thankfully, the typing spec currently makes very little use of equivalence. IIRC, the only place it shows up is in the description of `assert_type`. Put another way, I don't care as much about "equivalence" as I do about "assignability". The latter is much more foundational.

> I reject the description of ty's treatment of tuple[Never] and Never as equivalent as "special-casing" based on "implementation details"

I have a hard time seeing this as anything other than "special-casing based on implementation details". If I tell you that I've created a generic class `Foo` that has a single type variable `T` that is covariant, and I ask you "is `Foo[Never]` equivalent to `Never`", would you answer "definitely not!", or would you answer "I can't tell without more information; I'd need to see the implementation of `Foo`"?

I'm not arguing against innovation or public discussions about changes to the spec. But I don't think it serves the community well if the the foundational concepts in the type system are fluid or up for interpretation by each type checker. That's what I'm trying to avoid here. This discussion took me by surprise because it calls into question a concept that I thought was already well established in the typing spec — one that many other concepts build upon. I don't want a definition of assignability that depends on the subjective judgement of individuals. Nor do I want a definition that necessitates a multi-hour debate every time someone raises a question about "functional domains" or other semantic properties of a particular class. This is why it's important for the foundational rules to be clear and simple.

---

_Comment by @mikeshardmind on 2025-06-15 17:31_

@carljm @erictraut 

While I also don't want a definition where we keep having to re-clarify, I find it difficult to reconcile this with the amount of leeway typecheckers are currently given for constraint solving and inference, or places where we already know there are underspecified behaviors.

The following language would be easy to explain, covers the function domain problem, and sets a better ground for other things being worked on, and naturally encompasses the more involved definition that Elixir uses, and is proposed we accept.

"Consistent subtyping requires that if the super type is inhabited, that the subtype can be as well" (not is, but can be. it doesnt require a proof that some type can exist, only that there is no knowledge that prohibits such a type from existing)

This requires a slight change in the understanding of Never, from a consistent subtype of all types, to a representation of the absence of *the possibility* of a value consistent with the available type information (which happens to be more accurate)

This *would* also be a change in behavior as it implies the below error case:

```py
class A:
    def calc(self) -> int:
        ...

class B(A):
    def calc(self)  -> Never:  # error
        ...
```


I don't think it should be controversial that simply replacing a method expected to produce a value with one that never can does not satisfy a useful interface contract. (This is the domain of the method `calc` here, No inputs beyond bound self producing some int) 



---

_Comment by @Kroppeb on 2025-06-17 22:43_

@JelleZijlstra 

> from typing import assert_never, Never
> 
> class Foo(tuple[Never]): pass
> 
> def f(x: tuple[int | str]):
>     match x:
>         case (int(),):
>             pass
>         case (str(),):
>             pass
>         case _:
>             assert_never(x)
> 
> f(Foo())

Well there is at least a type error here is the instantiation of `Foo()` this way. If the `Never` is replaced with `int` it will still end up in the `assert_never` case yet still not produce any type errors.

---

_Comment by @JelleZijlstra on 2025-07-20 06:01_

I posted https://discuss.python.org/t/interactions-with-never-leading-to-undesirable-assignability/99445 to suggest a solution for this (spoiler: after more thinking I favor a solution similar to the Elixir one).

---

_Comment by @alwaysmpe on 2025-07-20 08:23_

I added a comment on Jelle's discussion but the short version I think is I agree with Eric, a sequence of length 1 of empty sets is a set containing a single member so is not in itself the empty set. There's no value that can materialize this at runtime (in python), but the type may be used (eg in a monad or as a generic argument). Other stuff I don't have a strong opinion on.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:05_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
