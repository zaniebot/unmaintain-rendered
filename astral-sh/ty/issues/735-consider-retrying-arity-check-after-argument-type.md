```yaml
number: 735
title: Consider retrying arity check after argument type expansion
type: issue
state: closed
author: dhruvmanila
labels:
  - calls
  - overloads
assignees: []
created_at: 2025-07-01T04:44:38Z
updated_at: 2025-09-12T08:40:09Z
url: https://github.com/astral-sh/ty/issues/735
synced_at: 2026-01-12T15:54:23Z
```

# Consider retrying arity check after argument type expansion

---

_@dhruvmanila_

Ref: https://github.com/astral-sh/ruff/pull/18996/files#r2172952014

--- 

This is a regression that was highlighted by the ecosystem check, which shows that we might need to rethink how we perform argument expansion during overload resolution. In particular, we might need to retry both `match_parameters` _and_ `check_types` for each expansion. Currently we only retry `check_types`.

The issue is that argument expansion might produce a splatted value with a different arity than what we originally inferred for the unexpanded value, and that in turn can affect which parameters the splatted value is matched with. In this example, the ternary operator produces a complex union type, which we expand when trying to call `range`. Our initial guess at its arity is "zero or more", but there are individual union elements with more precise arities (such as "exactly two"). `range`, via overloads and parameter defaults, can take in 1, 2, or 3 parameters. Our initial arity guess causes us to assign the splatted argument to all three parameters. But when we check the `(0, 100)` union element during argument expansion, we only have two values to provide for those three parameters.

For now, we have a workaround that pads out the splatted value with `Unknown` when we encounter this case, but a proper fix would retry parameter matching for each expanded union element.


```py
def _(batch_ids=(0, 100)) -> None:
    arg = batch_ids if isinstance(batch_ids, tuple) else (0, batch_ids)
    # revealed: (Unknown & tuple[Unknown, ...]) | (tuple[Literal[0], Literal[100]] & tuple[Unknown, ...]) | tuple[Literal[0], (Unknown & ~tuple[Unknown, ...]) | (tuple[Literal[0], Literal[100]] & ~tuple[Unknown, ...])]
    reveal_type(arg)
    range(*arg)
```

---

_Label `calls` added by @dhruvmanila on 2025-07-01 04:44_

---

_Label `overloads` added by @dhruvmanila on 2025-07-01 04:44_

---

_Comment by @carljm on 2025-07-01 05:56_

@erictraut I wonder if you have any insight on how you chose to handle this in pyright. The spec (in step 3, union expansion) says to "repeat from step 2" for each expanded set of arguments, and step 2 is just type checking (step 1 is arity checking). But it seems there are cases (e.g. a union of tuple types of different lengths, used as variadic argument) where union expansion might lead to different arity-check conclusions and different argument-parameter mappings.

---

_Comment by @erictraut on 2025-07-01 06:55_

This is partly related to [the thread](https://discuss.python.org/t/standardizing-argument-unpacking-behaviors-for-calls/80747) that I started several months ago in the typing forum. We haven't yet standardized how unpacked arguments are handled in call expressions — in particular, in situations where the unpacked iterable has an indeterminate length. The same is true for dictionary unpacking and keyword arguments. Since there is no standard currently, each type checker needs to decide how to handle this case.

With regard to the overload algorithm, step 1 performs _filtering_ based on arity checks. If the positional argument count is indeterminate because of an unpack operator, then arity-based filtering may not be able to eliminate any overloads. That's the case for the `range` constructor call in the example above.

Step 2 involves regular (non-overloaded) call evaluation. This is shorthand for "the type checker should do whatever it normally does here for non-overloaded calls". This is the part that's not yet standardized.

I was curious what mypy and pyright do for the code sample above. Here's a modified version that replaces the `range` constructor call with an overloaded function that has non-overlapping return types. 

```python
from typing import overload

@overload
def test(x: int, /) -> tuple[int]: ...
@overload
def test(x: int, y: int, /) -> tuple[int, int]: ...
# @overload
# def test(*args: int) -> tuple[int, ...]: ...
def test(*args: int) -> tuple[int, ...]: ...

def _(arg: tuple[int, ...]):
    result = test(*arg)
    reveal_type(result)
```

Interestingly, pyright reveals `Unknown` whereas mypy reveals `tuple[int]`. Mypy doesn't follow the algorithm in the typing spec in this case, and I think its result is unsound.

If we uncomment the third overload, then mypy and pyright agree because of step 4 in the overload algorithm.

---

_Added to milestone `Beta` by @carljm on 2025-08-19 14:50_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-08-22 13:44_

---

_Closed by @dhruvmanila on 2025-09-12 08:40_

---
