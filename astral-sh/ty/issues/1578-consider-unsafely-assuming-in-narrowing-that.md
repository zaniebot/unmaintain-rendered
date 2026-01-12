```yaml
number: 1578
title: "Consider unsafely assuming in narrowing that multiple inheritance of user types and builtin containers won't happen"
type: issue
state: open
author: RandomBrainCode
labels:
  - needs-decision
  - narrowing
assignees: []
created_at: 2025-11-17T14:44:47Z
updated_at: 2025-12-26T17:48:47Z
url: https://github.com/astral-sh/ty/issues/1578
synced_at: 2026-01-12T15:54:25Z
```

# Consider unsafely assuming in narrowing that multiple inheritance of user types and builtin containers won't happen

---

_@RandomBrainCode_

### Summary

Is it possible for Ty to infer the type of the Iterator Variable when Iterator is properly defined?

In the below example running, `area` is properly defined above

```
		if allowed:
			area: list[Area] = await user.get_allowed_areas()
		else:
			area: Area = await user.area_get()
```

but when I run `ty check`, I get the following
```
error[unresolved-attribute]: Object of type `object` has no attribute `get_model`
   --> app/routers/me.py:113:36
    |
112 |     if isinstance(area, list):
113 |         models: list[AreaModel] = [await a.get_model() for a in area]
    |                                          ^^^^^^^^^^^
114 |         return JSONResponse(content=loads(dumps([a.content() for a in models])), status_code=200)
    |
info: rule `unresolved-attribute` is enabled by default
```

I was able to resolve it by adding `a: Area` above line 113, but that causes ruff to throw warning `Local variable 'a' is annotated but never used`

One thing, I am not sure if this could be impacting it, but the variable `area` can actually be of type `Area` or type `list[Area]`, hence the `if isinstance(area, list):` at line 112.

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Comment by @RandomBrainCode on 2025-11-17 14:47_

**_CORRECTION_**: Annotating `a: Area` first did not resolve the issue:
```
error[unresolved-attribute]: Object of type `object` has no attribute `get_model`
   --> app/routers/me.py:114:36
    |
112 |     if isinstance(area, list):
113 |         a: Area  # noqa: F842
114 |         models: list[AreaModel] = [await a.get_model() for a in area]
    |                                          ^^^^^^^^^^^
115 |         return JSONResponse(content=loads(dumps([m.content() for m in models])), status_code=200)
    |
info: rule `unresolved-attribute` is enabled by default
```

---

_Label `narrowing` added by @carljm on 2025-11-17 15:13_

---

_Label `needs-decision` added by @carljm on 2025-11-17 15:15_

---

_Comment by @carljm on 2025-11-17 15:20_

Thanks for the report! What ty is (subtly) telling you here is that it is possible for there to exist a subclass of `Area` that is also a subclass of `list`, for example.

```py
class Hypothetical(Area, list[int]): ...
```

So because of that possibility, `isinstance(area, list): ...` does not actually guarantee that we have a `list[Area]`, it could be a list of anything. That's why ty infers `object` when you iterate over `area` after that check.

One alternative here is to use `not isinstance(area, Area)` in place of `isinstance(a, list)`; this [fixes the issue](https://play.ty.dev/748a6d1d-1b56-4e91-a857-0bec2c9bde43). 

Another alternative, if you don't ever subclass `Area`, is to mark it `@typing.final`, which will allow ty to make various more precise inferences based on the knowledge that it won't be subclassed.

This issue is related to #1566 and #1567 in that they are all examples of how ty is being "correct but pedantic" about narrowing, in a way which users may not prefer or be accustomed to. We do value "correct" highly, but I will leave this issue open as well, with `needs-decision` tag, so we can evaluate whether we need to consider doing something different.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-17 15:28_

---

_Renamed from "Infer type for the Iterator Variable in a For Loop" to "Consider unsafely assuming in narrowing that multiple inheritance of user types and builtin containers won't happen" by @carljm on 2025-11-17 16:48_

---

_Comment by @carljm on 2025-11-17 16:58_

Another thing we could try to do here (and in #1567 and similar issues) is add better explanatory diagnostics. This is tricky since these issues tend to manifest as just "unexpected `object` type inferred", but it might be possible by using a special variant of `object` in certain places (e.g. top materialization method return types) and then special-casing diagnostics involving that special `object` type.

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @RandomBrainCode on 2025-11-18 19:56_

@carljm Thanks for the reply!

I will give @typing.final a shot in the next couple of days and let you know if that resolves this scenario. The `Area` object will never be subclassed, so doing this should not cause any issues in my particular scenario.

---

_Comment by @carljm on 2025-12-26 17:48_

> Another thing we could try to do here (and in [#1567](https://github.com/astral-sh/ty/issues/1567) and similar issues) is add better explanatory diagnostics. This is tricky since these issues tend to manifest as just "unexpected `object` type inferred", but it might be possible by using a special variant of `object` in certain places (e.g. top materialization method return types) and then special-casing diagnostics involving that special `object` type.

I opened #2215 to track this idea.

---
