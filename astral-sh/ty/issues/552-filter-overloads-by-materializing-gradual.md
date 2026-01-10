```yaml
number: 552
title: Filter overloads by materializing gradual argument types for call evaluation
type: issue
state: closed
author: dhruvmanila
labels:
  - calls
  - overloads
assignees: []
created_at: 2025-05-30T15:30:10Z
updated_at: 2025-06-17T10:05:11Z
url: https://github.com/astral-sh/ty/issues/552
synced_at: 2026-01-10T02:08:20Z
```

# Filter overloads by materializing gradual argument types for call evaluation

---

_Issue opened by @dhruvmanila on 2025-05-30 15:30_

This corresponds to step 5 of the [overload call evaluation algorithm](https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation).

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-05-30 15:30_

---

_Label `overloads` added by @dhruvmanila on 2025-05-30 15:30_

---

_Added to milestone `Beta` by @dhruvmanila on 2025-05-30 15:31_

---

_Label `calls` added by @dhruvmanila on 2025-05-30 15:37_

---

_Comment by @carljm on 2025-06-10 14:18_

@dhruvmanila ran into an interesting case attempting to implement this. Consider this call:

```py
from typing import Any

def _(a: Any, b: Any):
    reveal_type("%s, %s" % (a, b))
```

And consider the overloads on `str.__mod__`:

```py
    @overload
    def __mod__(self: LiteralString, value: LiteralString | tuple[LiteralString, ...], /) -> LiteralString: ...
    @overload
    def __mod__(self, value: Any, /) -> str: ...
```

Following the [specified overload evaluation algorithm](https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation):

1. Arity does not eliminate any overload; both overloads remain candidates.
2. The call succeeds against either overload (`self` is `Literal["%s, %s"]`, `value` is `tuple[Any, Any]` which is assignable to `tuple[LiteralString, ...]`; both overloads remain candidates.
3. Step 3 is skipped since step 2 did not result in errors for all overloads.
4. Step 4 has no effect, since neither overload has a variadic parameter.
5. The first overload does not match all possible materializations of the arguments (`tuple[Any, Any]` can materialize to many types that aren't assignable to `LiteralString | tuple[LiteralString, ...]`), so no overloads are eliminated by the filtering. This means we have two remaining overloads, whose return types are not equivalent, therefore the spec says the return type should be `Any`.

But neither pyright nor mypy agree. [Pyright says](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcAUCQCYCmwUA%2BgBQCGAXIcQDRQBGrRcAlMxJQRUEJQBulRgBta8BJXoAiAKQBnThuVRVUJpy79%2BQA) the return type is `LiteralString`, which seems both wrong per the spec, and clearly unsafe with respect to the intended guarantees of `LiteralString` (since the string resulting from this interpolation could easily contain user-controlled text). [Mypy says](https://mypy-play.net/?mypy=latest&python=3.12&gist=cac5863e589273f5a9a71aa6b2ad4dc0) `str`, which seems more reasonable, but also doesn't seem to match the conclusion in the spec.

---

_Comment by @erictraut on 2025-06-10 15:07_

Yes, neither mypy nor pyright fully conform to the spec currently. It's a very difficult algorithm to implement in its entirety. For pyright, I'm tracking this noncompliance [here](https://github.com/microsoft/pyright/issues/10232).

The conformance tests are also quite incomplete when it comes to the overload matching algorithm. For example, they don't catch this particular deviation from the algorithm. You can complain about that to the guy who wrote these tests. ;)

There are a couple of reasons you're seeing the current behavior for pyright. The first is a bug (that I wasn't previously aware of) where it doesn't correctly handle argument types that contain an `Any` but are not actually `Any`. In your example, the type of the second argument is `tuple[Any, Any]`, and pyright wasn't handling this case correctly. I think mypy also handles this incorrectly.

The second reason is that pyright deviates from the spec slightly in that it does not always return `Any` after step 5 in the algorithm. If more than one match exists and all remaining return types are overlapping, it chooses the widest type. In your example, that's `str`. That's a much better result from the perspective of someone who is using a language server, but it is not conformant with the documented algorithm.

I plan to change this behavior in pyright at some point to fully match the spec, but I've been dragging my feet on it because the change is complicated and will cause some churn for pyright and pylance users.

---

_Comment by @dhruvmanila on 2025-06-13 04:09_

This seems like another case where we would diverge in the behavior following the spec (example from Pyright's test suite):

```py
from typing import Any, Literal, overload, reveal_type


@overload
def f(x: str, *, flag: Literal[True]) -> int: ...
@overload
def f(x: str, *, flag: Literal[False] = ...) -> str: ...
@overload
def f(x: str, *, flag: bool = ...) -> int | str: ...
def f(x: str, *, flag: bool = False) -> int | str:
    return 1


def check(any: Any):
    # ty: Any
    # Pyright, mypy: int
    reveal_type(f(any, flag=True))

    # ty: Any
    # Pyright, mypy: str
    reveal_type(f(any, flag=False))
```

Here, consider the first call `f(any, flag=True)`:
* Arity does not eliminate any overloads
* Type checking filters out the second overload because `True` is not assignable to `Literal[False]`
* Step 3 is skipped since step 2 did not result in errors for all overloads
* Step 4 has no effect, since neither overload has a variadic parameter
* First overload does not match all materializations of the arguments since `Any` can materialize into anything that's not a `str` and similar case for the third overload. So, no overloads are filtered via this step and the remaining overloads are 1 and 3. The return types `int` and `int | str` are not equivalent so the return type of the call is `Any`

But, it's only ty that infers it as `Any` while mypy and Pyright both infers `int`.

Similar case is for the second call (`f(any, flag=False)`).

This seems incorrect because what if it's `check(1)` in the above example so `any` is actually an integer and not guaranteed to be a `str`. For Pyright, this might be because of the second reason that @erictraut has provided above.



---

_Comment by @dhruvmanila on 2025-06-13 04:27_

> The second reason is that pyright deviates from the spec slightly in that it does not always return `Any` after step 5 in the algorithm. If more than one match exists and all remaining return types are overlapping, it chooses the widest type. In your example, that's `str`. That's a much better result from the perspective of someone who is using a language server, but it is not conformant with the documented algorithm.

This is an interesting point. I agree that this seems useful from that perspective. One way to improve the UX in an editor would be something that @carljm mentioned here (https://discuss.python.org/t/pragmatic-type-checker-defaults-for-untyped-decorators/94938/8) where the inferred type could still be `Any` but it would contain a "best guess type" which would be used only by the language server (for now, possibly the linter in the future).

---

_Comment by @erictraut on 2025-06-13 05:06_

> This seems like another case where we would diverge in the behavior following the spec

I think pyright and mypy are following the spec in this example — at least the intended meaning of the spec. I struggled with the wording of step 5 in the algorithm, and the spec is admittedly not crystal clear. If you have any suggestions for how to improve it, let me know.

It would probably help to work through a concrete example, so let's use your code above. The idea is that if you need to consider all possible materializations of `Any` (the first argument type) that are potentially assignable to `str` (the first parameter type). If all such materializations are also assignable to the corresponding parameter in subsequent overloads, then this argument will never distinguish from among the overloads — even if its type was statically known. In other words, we can assume here that if the user eventually replaces the `Any` with a type that is assignable to `str`, then the first overload will always be chosen, and third one will never be chosen. If the user eventually replaces the `Any` with a type that is not assignable to `str`, then it's an error condition, and no overloads will match. There will never be a situation where both overloads 1 and 3 are both chosen, so we can assume in this case that overload 1 is the winner.

And yes, I realize this is really complicated. It was implemented this way in mypy, which established the precedent.

> One way to improve the UX in an editor...

Yes, that's exactly what pyright already does already in the case where it returns an `Unknown`. You can see that in this screen shot. The revealed type is `Unknown` (consistent with the spec), but the presented completion suggestions come from the type `int | str`, which is the union of all unfiltered overloads.

![Image](https://github.com/user-attachments/assets/ae9a9c71-7666-4c54-b147-1c37eb5a90ce)

This is [implemented](https://github.com/microsoft/pyright/blob/8112830b0d05175fc169e790e2ee8686bc86e1ab/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L9429) using an internal variant of `Unknown` that I refer to as a "possible type" — as in, "the type is unknown, but it's possibly one of the following...". :)

---

_Comment by @dhruvmanila on 2025-06-13 06:03_

> It would probably help to work through a concrete example, so let's use your code above. The idea is that if you need to consider all possible materializations of `Any` (the first argument type) that are potentially assignable to `str` (the first parameter type). If all such materializations are also assignable to the corresponding parameter in subsequent overloads, then this argument will never distinguish from among the overloads — even if its type was statically known. In other words, we can assume here that if the user eventually replaces the `Any` with a type that is assignable to `str`, then the first overload will always be chosen, and third one will never be chosen. If the user eventually replaces the `Any` with a type that is not assignable to `str`, then it's an error condition, and no overloads will match. There will never be a situation where both overloads 1 and 3 are both chosen, so we can assume in this case that overload 1 is the winner.

I had this in the back of my mind that it _might_ be something related to the fact that the type of the first parameter is always `str` so it won't be required to be used to perform step 5. I think what you said makes sense, I'll have to think in terms of implementation based on what I already have.

---

(You can skip this if the implementation algorithm isn't relevant.)

For context, here's the current implementation of this step that is based on the concept of "top materialization" ([this document](https://github.com/astral-sh/ruff/blob/c5e01dc1884827156288b6d32662cdd7c51a78d3/crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md) has some context on what "top materialization" means):

Given the argument types `U1, U2, ..., UN` and the top materialization of each of the argument types `U1', U2', ..., UN'`, we go through each of the remaining overloads in order from top to bottom and check if the tuple containing the top materialized argument types `(U1', U2', ..., UN')` is assignable to the tuple of union of the parameter types up to that overload. It might be best to explain this using an example and I'm going to use a single argument example to best demonstrate this:

`overloaded.pyi`:

```pyi
from typing import Any, overload

@overload
def f(x: tuple[int, str]) -> int: ...
@overload
def f(x: tuple[int, Any]) -> int: ...
@overload
def f(x: Any) -> str: ...
```

```py
from typing import Any

from overloaded import f

def _(int_str: tuple[int, str], int_any: tuple[int, Any], any_any: tuple[Any, Any]):
    reveal_type(f(int_str))  # revealed: int
    reveal_type(f(int_any))  # revealed: int
    reveal_type(f(any_any))  # revealed: Any
```

Let's take the `f(int_any)` call and skipping directly to step 5 where all 3 overloads are remaining:
1. Tuple of top materialized argument types: `tuple[tuple[int, object]]` (replacing `Any` with `object` since it is in a covariant position)
2. For overload 1, check whether `tuple[tuple[int, object]]` is assignable to `tuple[tuple[int, str]]` which it isn't since `object` is not assignable to `str`, so we move to the next overload
3. For overload 2, check whether `tuple[tuple[int, object]]` is assignable to `tuple[tuple[int, Any]]` which it is, so we've found an overload where all materializations of the argument types are assignable to the parameter types
4. Filter the remaining overloads
5. Check whether the matching overloads (1 and 2) have equivalent type

The `f(any_any)` is an interesting example. Here, steps 1 and 2 from above will be the same except that the top materialized argument type is `tuple[tuple[object, object]]`, so we move to overload 3 where `tuple[tuple[object, object]]` is assignable to `tuple[Any]` but it's the last overload so none of the overloads are filtered out. Here, the return types are not equivalent for the matching overloads (all) so the return type is `Any`.

---

So, based on the above implementation, the reason we infer `Any` is that we are not intelligent about the fact that if the set of materialization of the argument type at a specific index that are assignable to the parameter type at the same index is same for all overloads, then it won't be participating in the filtering process.

---

_Comment by @erictraut on 2025-06-13 14:40_

Makes sense. Thanks for explaining. I like your concept of "top materialization" and "bottom materialization". I think that's useful, and I might borrow the idea in pyright's implementation.

---

_Comment by @dhruvmanila on 2025-06-16 03:07_

> I like your concept of "top materialization" and "bottom materialization".

Thanks, although this was introduced to us by the team working on gradual typing for Elixir :)

---

_Closed by @dhruvmanila on 2025-06-17 10:05_

---
