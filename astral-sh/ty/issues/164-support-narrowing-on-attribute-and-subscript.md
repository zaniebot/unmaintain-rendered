```yaml
number: 164
title: Support narrowing on attribute and subscript expressions
type: issue
state: closed
author: dhruvmanila
labels:
  - narrowing
  - attribute access
assignees: []
created_at: 2025-03-21T11:18:52Z
updated_at: 2025-06-17T09:07:48Z
url: https://github.com/astral-sh/ty/issues/164
synced_at: 2026-01-12T15:54:22Z
```

# Support narrowing on attribute and subscript expressions

---

_@dhruvmanila_

Currently, red-knot only supports narrowing on a name expression like `x` but not on attributes (`foo.x`) or subscripts (`foo[0]`).

For example the following is a false negative as detected in https://github.com/astral-sh/ruff/pull/16888#issuecomment-2742772151:
```py
from typing import Callable, Optional


class Config:
    formatting_function: Optional[Callable[[], None]] = None


def process(config: Config):
    reveal_type(config.formatting_function)  # revealed: () -> None | None
    if config.formatting_function:
        # red-knot: Object of type `None` is not callable [lint:call-non-callable]
        config.formatting_function()
```

---

_Comment by @InSyncWithFoo on 2025-03-21 13:02_

Narrowing on attributes and subscripts is inherently unsafe due to [TOCTOU](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use).

Nevertheless, both Mypy and Pyright do support narrowing subscripts to some extent:

(playgrounds: [Mypy](https://mypy-play.net/?mypy=1.15.0&python=3.13&flags=strict&gist=65644b8cbb0d1d6578e3091f3f9406f1), [Pyright](https://pyright-play.net/?pyrightVersion=1.1.397&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=CYUwZgBGAUBuBcEA2BLAzgFwNqYE4QB8IA5AewDsQBdAGghURXIwEoIBaAPhIpHgFgAUBBERcIWCACGSAPoYAngAcQcLAAYqbCAGIIeQj0pDRYidLmKValFpF6DRMscEnRKSLA1UBw06PFJGXllVS9NbQcMXCE3EQ8IL1tffwDzYKswrFtI-WjDZxAhIA))

```python
def f(v: list[str | None], i: int) -> None:
    reveal_type(v[0])  # str | None
    reveal_type(v[i])  # str | None

    if v[0]:
        reveal_type(v[0])  # str

    if v[i]:
        reveal_type(v[i])  # str | None
```


---

_Assigned to @sharkdp by @sharkdp on 2025-04-14 07:30_

---

_Unassigned @sharkdp by @sharkdp on 2025-04-14 07:30_

---

_Assigned to @sharkdp by @sharkdp on 2025-04-14 07:31_

---

_Comment by @sharkdp on 2025-04-14 07:32_

@mtshiba I can't assign this to you (maybe because you haven't commented in this thread), but I understand that you are working on this. I'm assigning myself as a placeholder, so that no-one else starts with this.

I think it would be good to start with narrowing on attribute expressions, and postpone narrowing on subscript expressions to a later point in time.

---

_Comment by @mtshiba on 2025-04-14 16:05_

Yes, I’m currently working on type narrowing for instance attributes, so if you could assign me for this issue, that would be great.

Subscript expressions support is also interesting. I'm wondering if there's a good idea to solve both at once.

---

_Assigned to @mtshiba by @sharkdp on 2025-04-14 16:17_

---

_Unassigned @sharkdp by @sharkdp on 2025-04-14 16:17_

---

_Comment by @carljm on 2025-04-25 15:31_

Hi @mtshiba! Just checking in on the status here and whether there is anything that it would be useful for us to review, even if it's just a planned approach or a test suite, to make sure we are all on the same page? We are hoping we can have some form of this feature landed within the next week or two; does that seem possible?

---

_Comment by @mtshiba on 2025-04-25 15:44_

Since I thought the fix for astral-sh/ruff#17595 was relevant to this issue (though not so relevant as it turned out), I have submitted [a PR](https://github.com/astral-sh/ruff/pull/17630) to fix that one first. Please check it.

A PR to solve this issue is also currently being prepared, but the basic feature should be ready within the next week.

---

_Renamed from "[red-knot] Support narrowing on attribute and subscript expressions" to "Support narrowing on attribute and subscript expressions" by @MichaReiser on 2025-05-07 15:26_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 16:01_

---

_Label `narrowing` added by @MichaReiser on 2025-05-08 17:51_

---

_Comment by @matthewlloyd on 2025-05-11 04:22_

Another example - the assert on the attribute should narrow the type here. As a Pyright user who makes heavy use of asserts and conditionals to narrow types and uses dataclasses everywhere, I have a *lot* of these in my codebase, over 1,000 instances 🤣:

```python
from dataclasses import dataclass

def black_box() -> str | None:
    return None

@dataclass
class A:
    a: str | None = None

b: str | None = black_box()
assert b is not None
# pyright - no error
# ty - no error
print(b.lower())

a = A()
a.a = black_box()
assert a.a is not None
# pyright - no error
# ty - warning[possibly-unbound-attribute]: Attribute `lower` on type `str | None` is possibly unbound
print(a.a.lower())
```

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:05_

---

_Removed from milestone `Alpha` by @MichaReiser on 2025-05-16 12:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-16 12:19_

---

_Comment by @lengau on 2025-05-22 16:07_

Just to add another wrinkle here: IMO the correct behaviour depends on whether the code might be multithreaded. Consider this example:

```python
import dataclasses
import time

@dataclasses.dataclass(frozen=True)
class Frozen:

    child: str | None


@dataclasses.dataclass
class Mutable:

    child: str | None


def should_narrow(x: Frozen):
    assert x.child is not None
    time.sleep(10)
    "".join(x.child)


def should_not_narrow(x: Mutable):
    assert x.child is not None
    time.sleep(10)
    "".join(x.child)
```

In the case of using the `Frozen` dataclass, even though it's not *technically* immutable, it can be treated as such and we can say with all the confidence we ever can in Python that another thread would not change the type of `x.child` during the sleep. On the other hand, the `Mutable` dataclass could have the value of `x.child` changed by another thread during sleep. 

Currently both mypy and pyright treat both cases as I described with `should_narrow`.

---

_Comment by @carljm on 2025-05-22 16:25_

@lengau You are right, but in general Python type checkers have made the decision to not attempt to catch this category of concurrency bug, and support forms of narrowing that are not necessarily sound in the face of concurrent mutation. Most Python objects are mutable, so respecting the possibility of concurrent mutation would dramatically reduce the usability of Python typing, including in code that is never run concurrently. 

Perhaps with the advent of free-threaded Python, this is a choice that the Python static type system will have to revisit in the coming years. It may make sense to consider an opt-in concurrent-strict mode.

But for now I think ty would hamstring its adoption and usability if it doesn't follow the lead of existing checkers. 

---

_Comment by @MichaReiser on 2025-05-22 17:09_

Do other type checkers narrow across await boundaries in async code because this isn't strictly a multi-threading problem. I still don't think we should invalidate narrowing. It seems to strict.

---

_Comment by @jelle-openai on 2025-05-22 17:19_

I believe Pyre tried to do this in a safe(ish) way by invalidating narrowing on any method call (because it could theoretically trigger code that reassigns the attribute). Even that is of course not safe in the presence of multithreading. But this approach turned out to be hard to use in practice; Carl might know more about how that worked out.

---

_Comment by @MichaReiser on 2025-05-22 17:21_

> I believe Pyre tried to do this in a safe(ish) way by invalidating narrowing on any method call (because it could theoretically trigger code that reassigns the attribute). Even that is of course not safe in the presence of multithreading. But this approach turned out to be hard to use in practice; Carl might know more about how that worked out.

That sounds very similar to what flowtype does. It confused a lot of people (including myself) and it required a few iterations to "allow" certain methods but I don't remember the details.

---

_Comment by @lengau on 2025-05-22 17:49_

@carljm that's completely fair.

Here's an entirely non-concurrent test case where `pyright` doesn't complain, but where the type narrowing should be reset:

```python
import dataclasses

@dataclasses.dataclass()
class Parent:

    child: str | None


def mutate_child(parent: Parent, new_child: str | None) -> None:
    parent.child = new_child


def tester(x: Parent):
    assert x.child is not None
    mutate_child(x, None)
    "".join([x.child])


parent = Parent(child="child")
tester(parent)
```

---

_Comment by @carljm on 2025-05-22 17:59_

Yes, all true, the issue can also occur with async, or with no concurrency at all just via function calls. (I think in practice it may be more likely with free threading than it has been otherwise, but we'll see.) I think the conclusion remains that this is the trade off Python type checkers have chosen and attempts to be more strict (like Pyre) have so far not gone over well. In practice the convenience of narrowing seems to outweigh the unsoundness, which doesn't come up that often.

Not a lot more to say about the Pyre experience: Pyre tried to roll out "safe" narrowing, and it was really unpopular. It was then rolled back to a slightly less conservative version that did the narrowing, but then invalidated it on function calls. This caused less irritation but people still found it pretty confusing. Meta's new type checker to replace Pyre (Pyrefly) seems to be fully following the lead of mypy and pyright, without any additional strictness around narrowing.

---

_Comment by @lengau on 2025-05-22 18:07_

That makes sense. This may be a good case for `info` level responses at a later date, especially in the LSP, but under those constraints it makes sense to follow other type checkers.

---

_Comment by @menzenski on 2025-06-01 11:23_

I got some `ty` errors this morning for the `invalid-argument-type` rule. I searched and found this issue - I believe mine might have the same cause. 

I am using `pydantic_extra_types.TimeZoneName` in the normal way, per the [docs](https://docs.pydantic.dev/latest/api/pydantic_extra_types_timezone_name/#pydantic_extra_types.timezone_name.TimeZoneName--normal-usage).

Tested on `ty` versions `0.0.1a6` and `0.0.1a7`.

```python
# model_test.py

from pydantic import BaseModel
from pydantic_extra_types.timezone_name import TimeZoneName

class TimeZonePolicyData(BaseModel):
    time_zone_name: TimeZoneName


class AmericaNewYorkTimeZonePolicy:

    def get_policy_data(self) -> TimeZonePolicyData:
        return TimeZonePolicyData(
            time_zone_name="America/New_York",
        )
```

Running `uv run ty check model_test.py` gives

```text
error[invalid-argument-type]: Argument is incorrect
  --> model_test.py:14:13
   |
12 |     def get_policy_data(self) -> TimeZonePolicyData:
13 |         return TimeZonePolicyData(
14 |             time_zone_name="America/New_York",
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `TimeZoneName`, found `Literal["America/New_York"]`
15 |         )
   |
info: rule `invalid-argument-type` is enabled by default
```

For comparison, running `uv run mypy model_test.py` gives: 

```text
model_test.py:14: error: Argument "time_zone_name" to "TimeZonePolicyData" has incompatible type "str"; expected "TimeZoneName"  [arg-type]
Found 1 error in 1 file (checked 1 source file)
```

(Seems related to https://github.com/pydantic/pydantic-extra-types/issues/316 ?)

EDIT: We're running both mypy and ty currently (on a new greenfield project, not in production). It's possible to ignore line issues in both, but the mypy ignore directive needs to be first.

This works:

```python
time_zone_name="America/New_York",  # type: ignore[arg-type]  # ty: ignore[invalid-argument-type]
```

This does not - mypy still complains:

```python
 time_zone_name="America/New_York",  # ty: ignore[invalid-argument-type] # type: ignore[arg-type]  
```

---

_Comment by @jelle-openai on 2025-06-01 17:25_

That's a completely unrelated issue; mypy and ty both give the same (apparently correct) error.

> This does not - mypy still complains

Mypy treats that as commenting out the `# type: ignore`, so it doesn't ignore the error.

---

_Comment by @menzenski on 2025-06-02 11:22_

> That's a completely unrelated issue

whoops, sorry!

---

_Closed by @sharkdp on 2025-06-17 09:07_

---
