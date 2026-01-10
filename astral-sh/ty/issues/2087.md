```yaml
number: 2087
title: "Expected `Iterable[Buffer]` when using `map(str, ...)`"
type: issue
state: closed
author: karlicoss
labels: []
assignees: []
created_at: 2025-12-18T20:18:01Z
updated_at: 2025-12-18T22:02:42Z
url: https://github.com/astral-sh/ty/issues/2087
synced_at: 2026-01-10T01:54:00Z
```

# Expected `Iterable[Buffer]` when using `map(str, ...)`

---

_Issue opened by @karlicoss on 2025-12-18 20:18_

### Summary

playground link: https://play.ty.dev/eab57770-64e1-44c8-940a-561b4340e9af

Consider the following program
```py
from typing import Sequence, reveal_type

def test(command: Sequence[str]) -> None:
    if isinstance(command, list):
        reveal_type(command)
        print(list(map(str, command)))

test(['echo', 'hello'])
```

This works in runtime, however since 0.0.3, ty is reporting a type error
```
$ uv run --with ty==0.0.3 -m ty check test.py 
info[revealed-type]: Revealed type
 --> test.py:5:21
  |
3 | def test(command: Sequence[str]) -> None:
4 |     if isinstance(command, list):
5 |         reveal_type(command)
  |                     ^^^^^^^ `Sequence[str] & Top[list[Unknown]]`
6 |         print(list(map(str, command)))
  |

error[invalid-argument-type]: Argument to function `__new__` is incorrect
 --> test.py:6:29
  |
4 |     if isinstance(command, list):
5 |         reveal_type(command)
6 |         print(list(map(str, command)))
  |                             ^^^^^^^ Expected `Iterable[Buffer]`, found `Sequence[str] & Top[list[Unknown]]`
7 |
8 | test(['echo', 'hello'])
  |
info: Matching overload defined here
    --> stdlib/builtins.pyi:3839:13
     |
3837 |     else:
3838 |         @overload
3839 |         def __new__(cls, func: Callable[[_T1], _S], iterable: Iterable[_T1], /) -> Self: ...
     |             ^^^^^^^                                 ----------------------- Parameter declared here
3840 |         @overload
3841 |         def __new__(cls, func: Callable[[_T1, _T2], _S], iterable: Iterable[_T1], iter2: Iterable[_T2], /) -> Self: ...
     |
info: Non-matching overloads for function `__new__`:
info:   (cls, func: (_T1@__new__, _T2@__new__, /) -> _S@map, iterable: Iterable[_T1@__new__], iter2: Iterable[_T2@__new__], /) -> Self@__new__
info:   (cls, func: (_T1@__new__, _T2@__new__, _T3@__new__, /) -> _S@map, iterable: Iterable[_T1@__new__], iter2: Iterable[_T2@__new__], iter3: Iterable[_T3@__new__], /) -> Self@__new__
info:   (cls, func: (_T1@__new__, _T2@__new__, _T3@__new__, _T4@__new__, /) -> _S@map, iterable: Iterable[_T1@__new__], iter2: Iterable[_T2@__new__], iter3: Iterable[_T3@__new__], iter4: Iterable[_T4@__new__], /) -> Self@__new__
info:   (cls, func: (_T1@__new__, _T2@__new__, _T3@__new__, _T4@__new__, _T5@__new__, /) -> _S@map, iterable: Iterable[_T1@__new__], iter2: Iterable[_T2@__new__], iter3: Iterable[_T3@__new__], iter4: Iterable[_T4@__new__], iter5: Iterable[_T5@__new__], /) -> Self@__new__
info:   (cls, func: (...) -> _S@map, iterable: Iterable[Any], iter2: Iterable[Any], iter3: Iterable[Any], iter4: Iterable[Any], iter5: Iterable[Any], iter6: Iterable[Any], /, *iterables: Iterable[Any]) -> Self@__new__
info: rule `invalid-argument-type` is enabled by default

Found 2 diagnostics
```

This only started happening in 0.0.3; 0.0.2 works without type error. Interesting enough, on 0.0.2 `reveal_type` still shows `Sequence[str] & Top[list[Unknown]]`, so felt like possible typeshed change? But doesn't look like there've been any recent typeshed syncs, so not sure what to make of it! https://github.com/astral-sh/ruff/commits/main/

Funny enough, if we change to `isinstance(command, list[str])`, this type checks in `0.0.3`

```
4 |     if isinstance(command, list[str]):
5 |         reveal_type(command)
  |                     ^^^^^^^ `Sequence[str]`
```

However 
- it's not allowed in runtime: `TypeError: isinstance() argument 2 cannot be a parameterized generic`
- the inferred type isn't correct, should be `list[str]`? (I guess this is irrelevant since ideally ty would flag this as invalid in runtime anyway)

Apologies if it's an existing issue, didn't really manage to find anything similar, and encountered independently in two separate projects, so figured worth reporting so people can find it in search.


### Version

ty 0.0.3/latest master

---

_Comment by @carljm on 2025-12-18 20:30_

Thanks for the report!

(BTW I'm seeing the same behavior on this snippet from ty 0.0.2 and 0.0.3, so I can't replicate your result that it passes on 0.0.2.)

This is intentional (and correct) behavior from ty; the behavior of other type checkers is unsound here and allows effectively bypassing variance entirely. It "works at runtime" in the case of the particular call you are making, but consider:

```py
def takes_sequence_of_int(x: Sequence[int]):
    if isinstance(x, list):
        # ty errors on the next line; if we narrowed to `list[int]`, we'd consider it ok
        x.append(99)

my_bools: list[bool] = [True, False]
# this is valid: `list[bool]` is assignable to `Sequence[int]`
takes_sequence_of_int(my_bools)
# now we have `[True, False, 99]` and the type checker thinks it is still `list[bool]`
```

What the type `Sequence[int] & Top[list[Unknown]]` is telling you is simply that a `Sequence[int]` which is a list at runtime is not necessarily a `list[int]`, it could be a `list[bool]`, or a list of any other subtype of `int`.

> Funny enough, if we change to `isinstance(command, list[str])`, this type checks in `0.0.3`

Yes, we should error on that, thank you! Filed https://github.com/astral-sh/ty/issues/2088

Closing this issue as "not planned" since ty is working as intended here.

---

_Closed by @carljm on 2025-12-18 20:30_

---

_Comment by @karlicoss on 2025-12-18 21:04_


> This is intentional (and correct) behavior from ty

Ah thanks @carljm your explanation makes sense. I tried rewriting original code to avoid `isinstance(..., list)` checks, since in my original case I was using it covariantly/read only anyway, so I rewrote 

```py
from typing import Sequence, reveal_type

def test(command: Sequence[str] | str) -> str:
    reveal_type(command)
    if isinstance(command, str):
        return command
    else:
        reveal_type(command)
        parts = list(map(str, command))
        return ' '.join(parts)

print(test('echo hello'))
print(test(['echo', 'hello']))
```

However it still failed:

```
error[invalid-argument-type]: Argument to function `__new__` is incorrect
  --> test.py:9:31
   |
 7 |     else:
 8 |         reveal_type(command)
 9 |         parts = list(map(str, command))
   |                               ^^^^^^^ Expected `Iterable[Buffer]`, found `Sequence[str] & ~str`
10 |         return ' '.join(parts)
   |
```

This one is different, right? I thought maybe it had to do with the fact that `Sequence[str] | str = Sequence[str]` (which is a bit of a special case in python), but replacing `str` with other type still results in similar issue.

It seems to work if I replace `command: list[str] | str`, so perhaps it's a case of https://github.com/astral-sh/ty/issues/1714, not sure.


> (BTW I'm seeing the same behavior on this snippet from ty 0.0.2 and 0.0.3, so I can't replicate your result that it passes on 0.0.2.)

Hmm sorry, not sure what was going on with `0.0.2` during local testing -- retried from scratch with `uvx ty==0.0.2 check test.py` and it failed as you say.


---

_Comment by @carljm on 2025-12-18 22:02_

Thanks for the follow up! That one definitely looks like a bug; it seems like we are not handling intersections correctly somehow when checking for compatibility with a protocol. I filed https://github.com/astral-sh/ty/issues/2091 to track this.

---
