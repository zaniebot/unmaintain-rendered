```yaml
number: 247
title: Support starred/splatted/unpacked arguments in function calls
type: issue
state: closed
author: dreid
labels:
  - bug
  - calls
assignees: []
created_at: 2025-05-07T16:30:44Z
updated_at: 2025-10-06T15:27:15Z
url: https://github.com/astral-sh/ty/issues/247
synced_at: 2026-01-12T15:54:22Z
```

# Support starred/splatted/unpacked arguments in function calls

---

_@dreid_

Status:
- [x] support single-starred arguments (https://github.com/astral-sh/ruff/pull/18996)
- [x] support single-starred arguments in calls to overloaded functions (https://github.com/astral-sh/ruff/pull/20223)
- [x] support double-starred arguments (https://github.com/astral-sh/ruff/pull/20430)


Original description:

<details>

### Summary

Minimal reproducer:

```py
def foo(initial_arg: str, *args: str) -> None:
    ...

foo(*["a", "b", "c"])  # No argument provided for required parameter `initial_arg` of function `foo` (lint:missing-argument) [Ln 4, Col 1]
foo(*[1, "a", "b"])  # No argument provided for required parameter `initial_arg` of function `foo` (lint:missing-argument) [Ln 4, Col 1]
```

https://play.ty.dev/4b8dc795-03b5-4fbf-a5c3-639f9518e093

I'd expect first the invocation to type check because the splatted args have compatible types to cover both the first argument and the var args.

</details>

---

_Renamed from "lint:missing-argument when splatting into a function that has a compatible positional argument." to "`lint:missing-argument` when splatting into a function that has a compatible positional argument." by @MichaReiser on 2025-05-07 16:34_

---

_Comment by @carljm on 2025-05-08 04:43_

Thanks for the report! Yeah, we don't support splatted arguments yet, but we should fix that soon.

---

_Label `bug` added by @carljm on 2025-05-08 04:43_

---

_Label `calls` added by @AlexWaygood on 2025-05-10 18:04_

---

_Comment by @GriceTurrble on 2025-05-18 03:26_

Similar scenario to watch for, which brought me here:

https://play.ty.dev/edb289aa-d398-4f51-b758-42f584ca80e1

```py
def something(foo, **others):
    pass

kwargs = {
    "foo": "one",
    "bar": "two",
}
something(**kwargs)
```

This throws the error:
```
No argument provided for required parameter `foo` of function `something` (missing-argument) [Ln 8, Col 1]
```

`foo` is part of the kwargs dict provided, but it's not recognized as being present.

---

_Renamed from "`lint:missing-argument` when splatting into a function that has a compatible positional argument." to "Support starred/splatted/unpacked arguments in function calls" by @sharkdp on 2025-05-19 14:00_

---

_Comment by @spapanik on 2025-05-28 21:28_

I am not sure if this is the same issue, I guess the root cause is the same. I am getting this error:

```py
error[unresolved-attribute]: Type `str` has no attribute `pop`
   --> src/eulertools/lib/utils.py:383:18
    |
381 |     prefix, response_key, *answers = line.split(maxsplit=2)
382 |     try:
383 |         answer = answers.pop()
    |                  ^^^^^^^^^^^
384 |     except IndexError:
385 |         answer = ""
    |
info: rule `unresolved-attribute` is enabled by default
```



---

_Comment by @carljm on 2025-05-30 00:38_

I think the most recent example here is actually https://github.com/astral-sh/ty/issues/191 -- the other examples do look like this issue.

---

_Comment by @dhruvmanila on 2025-06-03 09:45_

@spapanik The `unresolved-attribute` issue mentioned in https://github.com/astral-sh/ty/issues/247#issuecomment-2917656676 should be fixed by astral-sh/ruff#18438

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:32_

---

_Comment by @Andrej730 on 2025-06-13 19:35_

A note that currently other type checkers (pyright, mypy) do not check very precisely some cases of this and would allow errors like below to pass. So probably it would be okay for ty not to cover them too for a while.

```python
def test(a: int, b: int): ...

test(**{"a": 25, "b": 42, "c": 11})
test(*[5, 4, 23])
```

---

_Comment by @carljm on 2025-06-13 19:39_

Yes, I think that's perhaps not only a "for now" thing, but possibly a "for always" that we will have to match mypy/pyright's forgiving handling of splatted calls into non-variadic signatures, because I fear the backwards-compatibility impact would otherwise be too great. Perhaps at some point in the future we could explore more opt-in strictness here, but that's definitely not an immediate priority.

---

_Assigned to @dcreager by @carljm on 2025-07-01 06:31_

---

_Comment by @sharkdp on 2025-07-24 07:31_

@dcreager I understand that this is not yet finished, despite https://github.com/astral-sh/ruff/pull/18996 being merged? Do you think we can remove the "Missing support for starred/splatted/unpacked arguments in function calls" item from https://github.com/astral-sh/ty/issues/445 (after we cut a release)?

---

_Comment by @dcreager on 2025-07-24 13:53_

It should now work for calls to non-overloaded functions, but will likely produce invalid type inferences and diagnostics for calls to overloaded functions.

---

_Comment by @dcreager on 2025-07-24 13:55_

e.g. I updated the snippet to use tuples instead of lists (since we still aren't inferring a proper type for list literals), and it works as expected [[playground](https://play.ty.dev/f55d3567-da1f-4a0d-ac26-ae6963e4043b)]:

```py
def foo(initial_arg: str, *args: str) -> None:
    ...

foo(*("a", "b", "c"))

# error: [invalid-argument-type]
foo(*(1, "a", "b"))
```

---

_Comment by @hauntsaninja on 2025-07-24 20:48_

I would consider this a false positive on a non-overloaded function: https://play.ty.dev/92958251-1a2e-4238-b77c-83983bd7706f (yes, there can be errors here, but mypy and pyright accept it and this kind of thing is pretty common)

---

_Comment by @dcreager on 2025-07-24 20:52_

Ah sorry — I forgot to call out that https://github.com/astral-sh/ruff/pull/18996 only added support for positional splats, not keyword splats.

---

_Unassigned @dcreager by @carljm on 2025-08-15 01:26_

---

_Assigned to @dhruvmanila by @carljm on 2025-08-19 15:00_

---

_Closed by @dcreager on 2025-10-02 14:41_

---

_Comment by @sharkdp on 2025-10-06 08:15_

This has been closed, but the example given in https://github.com/astral-sh/ty/issues/445 still leads to errors (diagnostic code has changed since writing that example). This is the current behavior:
```py
def f(a: str, b: int): pass

opts = {"name": "foo", "n": 2}

# invalid-argument-type: Argument to function `f` is incorrect: Expected `str`, found `Unknown | str | int`
# invalid-argument-type: Argument to function `f` is incorrect: Expected `int`, found `Unknown | str | int`
f(**opts)
```

Is that being tracked somewhere else?

---

_Comment by @carljm on 2025-10-06 15:23_

I think that's what #1248 is about?

No other type checker really "supports" that example in any intelligent way. Pyright (in non-strict mode) avoids false positives by inferring the type of the dict as `dict[str, Unknown]`. Strict-mode pyright, pyrefly, and mypy all fail the same way we do.

---

_Comment by @AlexWaygood on 2025-10-06 15:26_

theoretically I guess you could bidirectionally infer the type of `opts` as being a synthesized `TypedDict` type due to the fact that it's kwarg-splatted into the `f()` call later on in the same scope. Sounds kinda hard though

---
