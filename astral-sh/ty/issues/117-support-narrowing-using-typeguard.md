---
number: 117
title: "Support narrowing using `TypeGuard`"
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - narrowing
  - typing semantics
assignees: []
created_at: 2025-04-21T21:53:46Z
updated_at: 2025-12-30T20:38:15Z
url: https://github.com/astral-sh/ty/issues/117
synced_at: 2026-01-10T01:51:13Z
---

# Support narrowing using `TypeGuard`

---

_Issue opened by @dhruvmanila on 2025-04-21 21:53_

Add support for narrowing on `TypeGuard` and `TypeIs`:

- [x] `TypeIs` https://github.com/astral-sh/ruff/labels/help%20wanted
- [x] `TypeGuard`

<details><summary>This will in turn support <code>builtins.callable</code>:</summary>
<p>

Add support for narrowing on `builtins.callable`, requires `TypeIs` support.

`builtins.pyi`:
```py
def callable(obj: object, /) -> TypeIs[Callable[..., object]]: ...
```

Otherwise, it causes a few false positives, like:
```py
def _(a: Any | None) -> None:
    # /private/tmp/mypy_primer/projects/bidict/bidict/_iter.py:50:34: Object of type `None` is not callable
    if callable(a):
        a()  # red-knot: Object of type `None` is not callable [lint:call-non-callable]
        reveal_type(a)  # revealed_type: Any | None
```

</p>
</details> 

---

_Comment by @AlexWaygood on 2025-04-21 22:08_

Hmm, won't this fall out naturally from TypeIs support? I'm not sure we'll need to add any special handling for `builtins.callable()` once we understand `TypeIs`.

It doesn't look like we have a dedicated issue for `TypeGuard` and `TypeIs` support right now, but we should maybe make one. It was one of the items listed in https://github.com/astral-sh/ruff/issues/13694, but that issue is now closed 

---

_Comment by @dhruvmanila on 2025-04-21 22:33_

> Hmm, won't this fall out naturally from TypeIs support? I'm not sure we'll need to add any special handling for `builtins.callable()` once we understand `TypeIs`.

Yeah, you're right. There's https://github.com/astral-sh/ruff/pull/16314 which is in draft.

---

_Renamed from "[red-knot] Support `builtins.callable`" to "[red-knot] Support narrowing using `TypeGuard` and `TypeIs`" by @dhruvmanila on 2025-04-21 22:36_

---

_Label `help wanted` added by @carljm on 2025-04-24 00:49_

---

_Comment by @carljm on 2025-04-24 00:51_

`TypeIs` at least should not be too difficult to build on top of our existing narrowing support; tagging "help wanted" for that.

`TypeGuard` may be somewhat more challenging, since it can fully override the previous type (like a cast), meaning we will have to extend our narrowing to support forms of narrowing that don't take the form of intersections. (Probably by changing narrowing constraints from a type to an enum where one variant is "a type to intersect with", another variant is "a typeguard type to override with", etc.)

---

_Comment by @InSyncWithFoo on 2025-04-24 01:30_

While working on astral-sh/ruff#16314, I encountered three major problems, all listed at [this comment](https://github.com/astral-sh/ruff/pull/16314#issuecomment-2676178735). I'm willing to resume my work if someone could give a few hints on how to handle them, especially the second.

As a note to myself, here's a small edge case example to add to the tests:

```python
def is_int(v: int | str) -> TypeIs[int]: ...

class C:
    a: int | str
    
    def __init__(self, a: int | str):
        self.a = a
        self.a_is_int = is_int(a)
```

```python
c = C('')

reveal_type(c.a)         # int | str
reveal_type(c.a_is_int)  # TypeIs[int]
                         # Or `TypeIs[a, int]`? `TypeIs[c.a, int]`? `TypeIs[C.a, int]`?

if c.a_is_int:
    reveal_type(c.a)     # ??
else:
    reveal_type(c.a)     # ??
```


---

_Comment by @carljm on 2025-04-24 04:54_

Ah, I wasn't even aware of that PR; I don't get notifications on draft PRs unless pinged. It looks like point 3 there is the same one I mentioned above: `TypeGuard` will require some significant additions to the current implementation of narrowing in red-knot. The differences in implementation are major enough that I would definitely suggest tackling them in separate PRs. I will try to find some time soon to review that PR and see if I can understand the other two points mentioned.

---

_Comment by @carljm on 2025-04-24 04:56_

> As a note to myself, here's a small edge case example to add to the tests

FWIW, neither pyright nor mypy support this case, and I don't think we need to either. It isn't clear in the spec, but I don't see any evidence in the spec or in the other typechecker implementations to suggest that type guards should have any effect on expressions outside the scope where the type guard is called.

---

_Comment by @InSyncWithFoo on 2025-04-27 01:26_

@carljm How about this, then?

```python
def f(a: int | str):
	class C:
		def __init__(self):
			self.a_is_int = is_int(a)

	reveal_type(C().a_is_int)  # TypeIs[a, int]

	if C().a_is_int:
		reveal_type(a)  # int?
```

In case it isn't clear, this is just a contrived example to demonstrate the second problem.

Neither Mypy nor Pyright supports this pattern, for presumably a very good reason. Something noteworthy is that the two type checkers differ in how they infer `.a_is_int`: Mypy considers it `bool`, while Pyright reveals `TypeIs[int]` (but the attribute isn't usable as a narrowing condition).

---

_Label `help wanted` added by @MichaReiser on 2025-05-07 15:18_

---

_Renamed from "[red-knot] Support narrowing using `TypeGuard` and `TypeIs`" to "Support narrowing using `TypeGuard` and `TypeIs`" by @MichaReiser on 2025-05-07 15:25_

---

_Label `narrowing` added by @AlexWaygood on 2025-05-10 18:05_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 11:01_

---

_Comment by @carljm on 2025-05-22 03:39_

@InSyncWithFoo I don't think that needs to be supported either.

I don't think we should support any pattern that would require carrying along with the `TypeIs` type the knowledge of what symbol or expression it was applied to (that is, any pattern where the call to a `TypeIs` function is separated from the actual conditional check). I think our implementation should simply look for calls that return `TypeIs` in a narrowing constraint, and directly check in the AST of the call (in the narrowing constraint resolution) to see what name (or attribute/subscript expression, once that PR lands) is narrowed by the call.

I realize that pyright does support some limited forms of separating the call from the check, within the same scope, but mypy does not, and there are no examples in the PEP or in the conformance test suite to suggest that this should be supported.

---

_Renamed from "Support narrowing using `TypeGuard` and `TypeIs`" to "Support narrowing using `TypeGuard`" by @carljm on 2025-08-15 14:57_

---

_Assigned to @sharkdp by @carljm on 2025-08-22 14:59_

---

_Comment by @sharkdp on 2025-09-19 15:13_

@dhruvmanila observed a false positive related to `TypeGuard` after some changes related to `**kwargs`. We should see that false positive disappearing when `TypeGuard` is implemented properly.

> https://github.com/mit-ll-responsible-ai/hydra-zen/blob/8f8d30622b85983e0f54004ea945858a72f32469/src/hydra_zen/structured_configs/_implementations.py#L2952. The parse_strict_dataclass_option function return a TypeGuard (https://github.com/mit-ll-responsible-ai/hydra-zen/blob/8f8d30622b85983e0f54004ea945858a72f32469/src/hydra_zen/structured_configs/_utils.py#L380-L386) to narrow the generic mapping into a concrete TypedDict.

---

_Unassigned @sharkdp by @MichaReiser on 2025-12-05 15:49_

---

_Assigned to @carljm by @carljm on 2025-12-05 15:50_

---

_Removed from milestone `Beta` by @carljm on 2025-12-16 21:08_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 21:08_

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:38_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:38_

---

_Comment by @jpgoldberg on 2025-12-21 05:31_

With respect to the M1 milestone (which I assume is *after* "stable") I would like to point out that `TypeIs` was introduced in Python 3.13, and 3.12's scheduled end-of-life isn't until late 2028. So like it or not `TypeGuard` will be a thing for nearly another three years.

---

_Comment by @carljm on 2025-12-21 05:34_

M1 is pre-stable

---

_Comment by @Avasam on 2025-12-21 16:46_

> I would like to point out that `TypeIs` was introduced in Python 3.13, and 3.12's scheduled end-of-life isn't until late 2028. So like it or not `TypeGuard` will be a thing for nearly another three years.

You make it sound like `TypeGuard` is deprecated or going away, but `TypeGuard` and `TypeIs` are both actively relevant and do something different (especially when it comes to assignability and the `False` branch): https://typing.python.org/en/latest/guides/type_narrowing.html#typeis-and-typeguard



---

_Comment by @jpgoldberg on 2025-12-22 21:36_

@Avasam said:

> You make it sound like TypeGuard is deprecated

That was not my intention,  but I see how what I wrote could be taken that way.
I had merely misunderstood the M1 milestone. But thank you for the reminder that `TypeGuard` is not going away.

It is true that I don't really grasp the difference between `TypeIs` and `TypeGuard`, but I don't need to for the (unnecessary) point I was trying to make. I was just trying to say that `TypeGuard` support is important. And it is because I had misunderstood the M1 milestone that I erroneously felt it needed to be said. And because I had misunderstood M1 as post-stable, I made incorrect presumptions about the reasons for such a choice.


---

_Comment by @MichaReiser on 2025-12-30 11:15_

@carljm should we close this, now that we have basic `TypeGuard` support or is there some work left to do here?

---

_Comment by @AlexWaygood on 2025-12-30 13:30_

Carl opened https://github.com/astral-sh/ty/issues/2267 for some followups, so I think we can close this as completed now

---

_Closed by @AlexWaygood on 2025-12-30 13:30_

---

_Comment by @carljm on 2025-12-30 20:38_

Yeah I thought this would be closed when I merged the PR -- I guess "Resolve(s)" doesn't match GitHub's auto-close regex :)

---
