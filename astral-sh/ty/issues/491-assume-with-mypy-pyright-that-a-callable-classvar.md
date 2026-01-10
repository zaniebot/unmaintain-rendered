```yaml
number: 491
title: Assume (with mypy/pyright) that a Callable ClassVar is a bound method descriptor
type: issue
state: open
author: carljm
labels:
  - calls
  - typing semantics
assignees: []
created_at: 2025-05-22T19:22:56Z
updated_at: 2025-12-19T11:21:03Z
url: https://github.com/astral-sh/ty/issues/491
synced_at: 2026-01-10T01:53:59Z
```

# Assume (with mypy/pyright) that a Callable ClassVar is a bound method descriptor

---

_Issue opened by @carljm on 2025-05-22 19:22_

Currently we assume that Callable types do not implement the descriptor protocol -- specifically, the bound method descriptor behavior that function and lambda objects implement, to bind the first argument to the receiver instance.

This is a reasonable choice, since the Callable type definitely includes objects which are not bound method descriptors (e.g. you can assign a callable instance or protocol or staticmethod object -- none of which are bound method descriptors -- to a callable type). If you consider the bound-method descriptor behavior as additional behavior that can be provided by a subtype, then our current behavior makes the most sense: `Callable` implies only `__call__`, function and lambda objects are subtypes of Callable which add the `__get__` method as well.

The problem with this is that adding a bound-method `__get__` is Liskov-incompatible -- it changes the behavior in an incompatible way, that makes the bound method descriptor object not safely substitutable (as a class attribute) for a callable with the same signature that is not a bound-method descriptor.

This is a fundamental problem with the Python type system, and there's not much we can do about it. Any choice we make in this area will be unsound; the only sound option would be a wholesale change to Python typing to (in general) not consider adding a `__get__` method to be a Liskov-compatible change in a subtype; this is not practical.

Given that we can't be sound, we may want to at least be _compatible_ and match the behavior of existing type checkers. I've created this "test suite" to check the behavior of existing type checkers:

<details>

```py
from typing import Callable, ClassVar, assert_type

class Descriptor:
    def __get__(self, instance: object, owner: type) -> int:
        return 1

def impl(self: "C", x: int) -> int:
    return x

class C:
    descriptor: Descriptor = Descriptor()
    classvar_descriptor: ClassVar[Descriptor] = Descriptor()

    callable: Callable[["C", int], int] = impl
    classvar_callable: ClassVar[Callable[["C", int], int]] = impl
    static_callable: Callable[["C", int], int] = staticmethod(impl)
    static_classvar_callable: ClassVar[Callable[["C", int], int]] = staticmethod(impl)

c = C()

# Establish a baseline that type checkers generally respect the descriptor
# protocol for values assigned in the class body, whether annotated with
# ClassVar or no:
assert_type(c.descriptor, int)
assert_type(c.classvar_descriptor, int)

# The calls and assignments below are all correct per runtime behavior;
# if a type checker errors on any of them and expects a different
# signature, that indicates unsound behavior. Note that the static_*
# variants are annotated exactly the same as the non-static variants,
# but have different runtime behavior, because Callable does not
# distinguish descriptor vs non-descriptor. Thus, it's unlikely that any
# type checker can get all of these correct.

# If a type-checker assumes that callable types are not descriptors,
# it will (wrongly) error on these calls and assignments:

c.callable(1)
c.classvar_callable(1)

x1: Callable[[int], int] = c.callable
x1(1)
x2: Callable[[int], int] = c.classvar_callable
x2(1)

# If a type-checker assumes that callable types are descriptors,
# it will (wrongly) error on these calls and assignments:

c.static_callable(C(), 1)
c.static_classvar_callable(C(), 1)

y1: Callable[["C", int], int] = c.static_callable
y1(C(), 1)
y2: Callable[["C", int], int] = c.static_classvar_callable
y2(C(), 1)

# Now let's look specifically at annotated `__call__` attributes:

def cm_impl(self: "CallMethod", x: int) -> int:
    return x

class CallMethod:
    __call__: Callable[["CallMethod", int], int] = cm_impl

def cmc_impl(self: "CallMethodClassVar", x: int) -> int:
    return x

class CallMethodClassVar:
    __call__: ClassVar[Callable[["CallMethodClassVar", int], int]] = cmc_impl

def cms_impl(self: "CallMethodStatic", x: int) -> int:
    return x

class CallMethodStatic:
    __call__: Callable[["CallMethodStatic", int], int] = staticmethod(cms_impl)

def cmcs_impl(self: "CallMethodClassVarStatic", x: int) -> int:
    return x

class CallMethodClassVarStatic:
    __call__: ClassVar[Callable[["CallMethodClassVarStatic", int], int]] = staticmethod(cmcs_impl)

# Again, all of these are correct per runtime behavior; type checker
# errors indicate an unsound interpretation:

# Type checkers which assume callables are not descriptors will (wrongly)
# error on these:

CallMethod()(1)
cm: Callable[[int], int] = CallMethod()
cm(1)

CallMethodClassVar()(1)
cmc: Callable[[int], int] = CallMethodClassVar()
cmc(1)

# Type checkers which assume callables are descriptors will (wrongly)
# error on these:

CallMethodStatic()(CallMethodStatic(), 1)
cms: Callable[["CallMethodStatic", int], int] = CallMethodStatic()
cms(CallMethodStatic(), 1)


CallMethodClassVarStatic()(CallMethodClassVarStatic(), 1)
cmcs: Callable[["CallMethodClassVarStatic", int], int] = CallMethodClassVarStatic()
cmcs(CallMethodClassVarStatic(), 1)

```

</details>

It seems that both [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDSHkDOjAakSPUc9XgPrwJqAKCEBjJoygARao1EgkCGLgBcQqBqgATasCi9eaajAMAKRtRLB6qRjCIpR1FVDAUAVtVEx6YAO4oPC4C1ACUUAC0AHyYKDBqmolQIMYAriAoUACMIjp6WAgk5pbALgBEBGX0AB4uqDDh0bHx6popMOmZ1SLiXJIECZo6cgpKqtKy8orKIFAAvBMj07imoa0avcwAbuy8w1NjIC4EEmwgANoyS4cAuvOLBzOrIomipORUzoTvlDTn5xUqs0bjY4ncFgUSOsoJtGDsQLw3mRfl8Tn0zudiMjPv9AaCYCDgeDMNgoYk7EQYEhRIifp9jnS-gDKvjCfViRSqaIIMYABZgLSmSFrcn2LmIiTw2nYmjHU7sTGM6i4lnA1kcsXUnkwfmC4U9e4EZ5CADEUAAohSqEhGLyoEQoBQuJZUNRYLzKbBEG7RLyvABrHiSIyBEDvODJWSCbzut37UYzU1QBDgZSiMAkKDAXBQHYkVKye3MJBoQJaWKxmESR0CuD0Px%2BnU8e0oFBgMXUct%2BJA6pNo5hnVyzNtqPo8EwhUyiAB08eWHGaazHfEnM9hUrnh3xayTABU-TD3pIHOW%2BiWUDy4pIKJZ-PaUvayDDcCkY4JZiBUnEsG6bx6tkguAANxJkgegOiEMJ%2BqIgazDw4AgJIYCZA4EZgHoTbQCeUDUNU0YwMe2hgcAPDUHESaMOelLpHQ7qeqgWjUpShZfowYBfuWf5EABuDTlAABy7ZujqnpNlAnLUrwABUSbwkgDgEfeboOG2HblrhRDeCQEZiYwRA8kWlZtigEQSaIubsPJV60EmFCpPg-5xsRpFxMkX5UgZXE8QuN5vKkFjfDKcZgIWqlJoxdioGgqQ2nam4zLmkjGRE8W8VA%2B7%2BTYMAAOSSF%2BJBIIG2l0fgqFJpBvoBs2byZEYpVPuhsYBemICvjA04iGaACS4FeoIESVTBzZ9KkPKSCJ%2BBIh8NB9YW7Buqp2iTAmuCMDZZo9lA3ZPqYfjgOg2nhPBObIU1PpHi2p7FqWl4EWoYjTlNKKmFkaxrpKuxPZ8L07tUWQMkF-zsqy9xrkqQh-T9EMAEwA9NyrnMDwKg49H0Il9NAw1DSY9fas0DdBsFFowo2FhNh5BbNx4PqliHrZg%2BDbZmu37Wgh04a1J2ZE2zUXdhZ43WRd09NOZnSvDphGqE9CvQ9Yvrp9SqS6sMs7nA-2BfDKpAkj7Io-L4Pq8r0vZGscCw5rKLa6yIMLDO8to%2BLKJCObxuq51Al3jQOWSCQYBgP64nRmBTFkBGnoqe2zHlgABgYU0GDH9owDACh2TAsj3XkMIQLwkLFFY5RYiQACyfICkCtSLpEMT1IMGjtJ0UDdGI1bF2XOoCvX%2BjiwYcNW8y7wd7qOtgrbOd56SuS6DnNL5xYhdQBUQ-l1o-asOwld1HEjS13E3eNxkzc9G3K%2Bd2v8pHNC8fvH3DDogqxcD8vZDDwK69nKPBLqqDEBz1PQhs7ckYJPQoBdShL3bqvAAypqUQW9q5NDrtCQ%2BXQT59E1m-LQsDKTUm7jfMgd8n44kHq-GBcCv5sjBPcMy2pdRTggCA-UgCZ7clEEw0k4Ci5n11B-dgOCuQIPqLvZoB80hHxbrCTBq8%2BEgAEXg6%2BvdeBygfhcYhTIX6lxkZfeR8CbZEhoXAuhAoGHsNASQHcZoACCaAiCoE4A1DCfoArzWfK1Lw%2BB3xuW-J5ag-5AIgCArNKCVUQBJmOohWIjE3gZxbFAVi7EUDlnqDwFMxhcHIXumaXc3oQlDUiQ2akdoRoGQxnNB8i1aaSCZlAFmyE2ZwDWGaCJrhubOOcCIKB59VhQ25P3EhutqELC6fQt6EBsYjPfpfHpss2H9KZIMgkhoeFTNUc8Nh2Nsm5MGrBapvIinE1JhTeG1M4zLXnNUpAO09r1MOuEzmsxTo8w6UISZ2C4E9LebolWpsxCMPmQjTRWDdGUPHl8j5YzGCSxWe83Bogfmy06TC2R3zQjQrIefFFEL3ZsMYAClUyKdEUP0XrYZhLVGor%2Bew9FWjMVErhQitYQA) and [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=2935159a8183d1929c8a78b1e586059b) implement a heuristic in which callable class attributes explicitly annotated as `ClassVar` are assumed to be bound-method descriptors, and those not so annotated are assumed not to be. (Note that this distinction is specific to callable types; both mypy and pyright are in general happy to execute the descriptor protocol on class attributes not explicitly annotated as `ClassVar`.) Mypy appears to add one additional wrinkle: `__call__` attributes annotated with a callable type are always assumed to be bound-method descriptors, unlike other attributes. Pyright doesn't implement this bit.

[Pyrefly](https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDSHkDOjAakSPUc9XgPrwJqAKCEBjJoygARao1EgkCGLgBcQqBqgATasCi9eaajAMAKRtRLB6qRjCIpR1FVDAUAVtVEx6YAO4oPC4C1ACUUAC0AHyYKDBqmolQIMYAriAoUACMIjp6WAgk5pbALgBEBGX0AB4uqDDh0bHx6popMOmZ1SLiXJIECZo6cgpKqtKy8orKIFAAvBMj07imoa0avcwAbuy8w1NjIC4EEmwgANoyS4cAuvOLBzOrIomipORUzoTvlDTn5xUqs0bjY4ncFgUSOsoJtGDsQLw3mRfl8Tn0zudiMjPv9AaCYCDgeDMNgoYk7EQYEhRIifp9jnS-gDKvjCfViRSqaIIMYABZgLSmSFrcn2LmIiTw2nYmjHU7sTGM6i4lnA1kcsXUnkwfmC4U9e4EZ5CADEUAAohSqEhGLyoEQoBQuJZUNRYLzKbBEG7RLyvABrHiSIyBEDvODJWSCbzut37UYzU1QBDgZSiMAkKDAXBQHYkVKye3MJBoQJaWKxmESR0CuD0Px+nU8e0oFBgMXUct+JA6pNo5hnVyzNtqPo8EwhUyiAB08eWHGaazHfEnM9hUrnh3xayTABU-TD3pIHOW+iWUDy4pIKJZ-PaUvayDDcCkY4JZiBUnEsG6bx6tkguAANxJkgegOiEMJ+qIgazDw4AgJIYCZA4EZgHoTbQCeUDUNU0YwMe2hgcAPDUHESaMOelLpHQ7qeqgWjUpShZfowYBfuWf5EABuDTlAABy7ZujqnpNlAnLUrwABUSbwkgDgEfeboOG2HblrhRDeCQEZiYwRA8kWlZtigEQSaIubsPJV60EmFCpPg-5xsRpFxMkX5UgZXE8QuN5vKkFjfDKcZgIWqlJoxdioGgqQ2nam4zLmkjGRE8W8VA+7+TYMAAOSSF+JBIIG2l0fgqFJpBvoBs2byZEYpVPuhsYBemICvjA04iGaACS4FeoIESVTBzZ9KkPKSCJ+BIh8NB9YW7Buqp2iTAmuCMDZZo9lA3ZPqYfjgOg2nhPBObIU1PpHi2p7FqWl4EWoYjTlNKKmFkaxrpKuxPZ8L07tUWQMkF-zsqy9xrkqQh-T9EMAEwA9NyrnMDwKg49H0Il9NAw1DSY9fas0DdBsFFowo2FhNh5BbNx4PqliHrZg+DbZmu37Wgh04a1J2ZE2zUXdhZ43WRd09NOZnSvDphGqE9CvQ9Yvrp9SqS6sMs7nA-2BfDKpAkj7Io-L4Pq8r0vZGscCw5rKLa6yIMLDO8to+LKJCObxuq51Al3jQOWSCQYBgP64nRmBTFkBGnoqe2zHlgABgYU0GDH9owDACh2TAsj3XkMIQLwkLFFY5RYiQACyfICkCtSLpEMT1IMGjtJ0UDdGI1bF2XOoCvX+jiwYcNW8y7wd7qOtgrbOd56SuS6DnNL5xYhdQBUQ-l1o-asOwld1HEjS13E3eNxkzc9G3K+d2v8pHNC8fvH3DDogqxcD8vZDDwK69nKPBLqqDEBz1PQhs7ckYJPQoBdShL3bqvAAypqUQW9q5NDrtCQ+XQT59E1m-LQsDKTUm7jfMgd8n44kHq-GBcCv5sjBPcMy2pdRTggCA-UgCZ7clEEw0k4Ci5n11B-dgOCuQIPqLvZoB80hHxbrCTBq8+EgAEXg6+vdeBygfhcYhTIX6lxkZfeR8CbZEhoXAuhAoGHsNASQHcZoACCaAiCoE4A1DCfoArzWfK1Lw+B3xuW-J5ag-5AIgCArNKCVUQBJmOohWIjE3gZxbFAVi7EUDlnqDwFMxhcHIXulA8+qwobcn7iQ3W1CFjZPoW9CA2NSnv0vrk2WbCClMiKQSQ0PDqmqOeGwyprTsFwNyVUnpuDRAq1NmIRhDSEaaKwboyh49+m6I6YwyW3T5km1liIfpsiVlLLIefTZvTVnlPYeMlU3S9mDJmcjEppydH7NGew7ZWjdk3MGcM2WQA) currently implements the same thing [we do](https://play.ty.dev/b60ede58-af47-437a-8af0-a492f792095a): callables are never assumed to be bound-method descriptors.

---

_Label `calls` added by @carljm on 2025-05-22 19:22_

---

_Label `typing semantics` added by @carljm on 2025-05-22 19:22_

---

_Comment by @carljm on 2025-05-22 19:56_

https://github.com/astral-sh/ruff/pull/18167 has some tests that may be useful to adapt, if/when we decide to do this.

---

_Comment by @carljm on 2025-05-22 21:02_

Posted for discussion at https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938

---

_Comment by @KotlinIsland on 2025-06-07 08:50_

from #600

### Summary

descriptors can be unsafe:
```py
class A: 
    pass

class B(A):
    def __get__(self, *_) -> 1:
        return 1

class C:
    a: A = B()

C.a  # static: A, runtime: 1
```
we can avoid this be reporting an error on the signature of `__get__`: "`__get__`s return type must be assignable to the super classes `__get__` (defaulting to the type of the superclass in it's absense)"

unfortunatly, `FunctionType` commits this sin, and it is an invalid subtype of `Callable`:

```py
class A:
    c: Callable[[object], None] = lambda self: None
A().c  # static: (object) -> None, runtime: () -> None
```

see: https://play.ty.dev/79255fa6-667e-4fa8-b6f2-32225f783bbd

# prior art
basedmypy resolved this problem fairly well by have special cased errors for these assignments to classes for `FunctionType` and `Callable`. the issue is that the corpus of existing code is written under the broken mypy semantics that all `Callable`s are actually `FunctionType`s


perhaps ideally we could just report that `FunctionType` is incompatible with `Callable`:

```py
_: Callable[[], None] = lambda: None  # error: FunctionType is incompatible with Callable, please try again in a few minutes
```


### Version

_No response_

---

_Comment by @KotlinIsland on 2025-06-07 08:51_

basedmypy has a lot of work to resolve cases like this

---

_Comment by @sharkdp on 2025-07-28 09:39_

See https://github.com/astral-sh/ty/issues/908 for a case where this leads to a `unsupported-operator` diagnostic for [`torch.Tensor.__pow__`](https://github.com/pytorch/pytorch/blob/f3913ea641d871f04fa2b6588a77f63efeeb9f10/torch/_tensor.py#L1084-L1092).

---

_Comment by @carljm on 2025-08-29 16:30_

In general, this means that methods decorated with a decorator returning a `Callable` type will not work (they won't bind as methods). This also causes problems with SymPy, which decorates many methods, see https://github.com/astral-sh/ruff/pull/20140

I think at the very least here we will need to propagate our "is function-like" callable-type flag through function decorators annotated to return `Callable` types.

This may be the cause of enough false positives that we need to consider addressing it for the beta?

---

_Added to milestone `Beta` by @carljm on 2025-08-29 16:34_

---

_Assigned to @sharkdp by @carljm on 2025-09-19 14:57_

---

_Renamed from "Assume (with mypy/pyright) that a Callable ClassVar is a bound method descriptor?" to "Assume (with mypy/pyright) that a Callable ClassVar is a bound method descriptor" by @carljm on 2025-09-19 15:47_

---

_Closed by @sharkdp on 2025-10-14 12:27_

---

_Comment by @sharkdp on 2025-10-14 12:32_

I chose to close this for now. I implemented two heuristics to improve the situation here. They are described at the end of [this document](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md). Together, they removed a lot of diagnostics across the ecosystem (including 3000 diagnostics on `sympy`). And they solve all of the user-reported bugs that were linked to this ticket.

I also tried to generally treat `ClassVar`-qualified `Callable` attributes as bound-method descriptors (as described in the original post here), but that had zero impact.

If there are other use cases that I haven't considered, please feel free to comment here and we can reopen the ticket.

---

_Comment by @carljm on 2025-10-24 18:09_

A user gave this use case in Discord: https://gist.github.com/ryanhiebert/7751ad1efac64f735ec231b2ce5b6cc6

It doesn't appear to be covered by the fix here?

I suspect we may need to follow other type checkers here and make this assumption more broadly, since using an intersection with `FunctionType` isn't really a practical option yet, even in ty (unless we were to make `ty_extensions` exist at runtime), and certainly not for code annotated for compatibility with other type checkers too.

Reopening this since we don't yet fully "assume (with mypy/pyright) ..." and it seems we still need to evaluate the option of doing so.

---

_Reopened by @carljm on 2025-10-24 18:09_

---

_Removed from milestone `Beta` by @carljm on 2025-10-24 18:10_

---

_Comment by @sharkdp on 2025-10-29 15:30_

Based on a discussion with Carl: It looks like the remaining inconsistency between ty and mypy/pyright/pyrefly comes from the fact that other type checkers assume that all `FunctionType` attributes exist on a `Callable`, for example:
```py
from typing import Callable, reveal_type


def _(c: Callable):
    reveal_type(c.__name__)  # mypy/pyright/pyrefly: str, ty: unresolved-attribute
```
As a consequence of this, other type checkers also assume that (the `FunctionType` version of) `__get__` exists on every `Callable`, but they do *not* actually call that `__get__` method (in all cases) when accessing a `Callable` attribute. This is of course inconsistent, but we should consider doing the same.

```py
from typing import Callable, reveal_type


class C:
    f: Callable[[int], str]


C.f(1)  # mypy/pyright/pyrefly: this is fine

c = C()
c.f(1)  # mypy/pyright/pyrefly: this is also fine

C.f.__get__(c, C)  # mypy/pyright/pyrefly: this is also fine?!?
```

---

_Comment by @carljm on 2025-11-06 16:13_

It seems like there is a case in `discord.py` (with an `on_error` attribute that is assigned dynamically from a callable type) that comes up in #21139 that looks like it also might be related to this? Needs more exploration to minimize an example and compare our behavior to pyright/mypy/pyrefly.

From @dhruvmanila :

https://github.com/Rapptz/discord.py/blob/8f2cb6070026bd2be2fd1fe35f978b408490879b/discord/app_commands/commands.py#L807 here is where ty is raising an error after https://github.com/astral-sh/ruff/pull/21139

Because of this Callable type alias: https://github.com/Rapptz/discord.py/blob/8f2cb6070026bd2be2fd1fe35f978b408490879b/discord/app_commands/commands.py#L78-L78

which is being assigned here: https://github.com/Rapptz/discord.py/blob/8f2cb6070026bd2be2fd1fe35f978b408490879b/discord/app_commands/commands.py#L1850


---

_Comment by @carljm on 2025-11-06 16:19_

I filed https://github.com/astral-sh/ty/issues/1495 for the somewhat-separate issue of modeling all `FunctionType` attributes existing on all `Callable` types.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:41_

---

_Comment by @spaceone on 2025-12-18 11:30_

Another example for this issue:
```sh
$ ty check t.py 
t.py:4:12: error[unresolved-attribute] Object of type `(...) -> int` has no attribute `__name__`
Found 1 diagnostic
$ cat t.py
from typing import Callable

def foo(cb: Callable[..., int]):
    return cb.__name__

```

---

_Comment by @sharkdp on 2025-12-18 11:34_

> Another example for this issue:

I don't think so, this is different. I wrote a [FAQ entry](https://github.com/astral-sh/ty/pull/2055) just this morning explaining why that is happening. The PR is not merged yet, but you can see a preview of the rendered version [here](https://github.com/astral-sh/ty/blob/david/callable-attribute-faq-entry/docs/reference/typing-faq.md#why-does-ty-say-callable-has-no-attribute-__name__).

---

_Comment by @spaceone on 2025-12-18 12:15_

ok, I outsourced it into https://github.com/astral-sh/ty/issues/2065

---

_Unassigned @sharkdp by @sharkdp on 2025-12-19 11:21_

---
