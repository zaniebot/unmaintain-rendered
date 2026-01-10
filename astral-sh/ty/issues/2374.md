```yaml
number: 2374
title: "`isinstance(..., dict)` and top materialization result in false positive on `.get()` call"
type: issue
state: open
author: yilei
labels: []
assignees: []
created_at: 2026-01-07T04:21:12Z
updated_at: 2026-01-07T18:15:14Z
url: https://github.com/astral-sh/ty/issues/2374
synced_at: 2026-01-10T01:56:41Z
```

# `isinstance(..., dict)` and top materialization result in false positive on `.get()` call

---

_Issue opened by @yilei on 2026-01-07 04:21_

### Summary

Given the following code:

```python
def foo(body: object) -> str | None:
    if isinstance(body, dict):
        value = body.get("key")  # reveal_type(body) shows `Top[dict[Unknown, Unknown]]`
        if isinstance(value, str):
            return value
    return None
```

`ty` reports:

```
error[invalid-argument-type]: Argument to bound method `get` is incorrect
 --> isinstance_dict_narrowing.py:3:26
  |
1 | def foo(body: object) -> str | None:
2 |     if isinstance(body, dict):
3 |         value = body.get("key")
  |                          ^^^^^ Expected `Never`, found `Literal["key"]`
4 |         if isinstance(value, str):
5 |             return value
  |
info: Matching overload defined here
    --> stdlib/builtins.pyi:3015:9
     |
3013 |     # Positional-only in dict, but not in MutableMapping
3014 |     @overload  # type: ignore[override]
3015 |     def get(self, key: _KT, default: None = None, /) -> _VT | None:
     |         ^^^       -------- Parameter declared here
3016 |         """Return the value for key if key is in the dictionary, else default."""
     |
info: Non-matching overloads for bound method `get`:
info:   (self, key: _KT@dict, default: _VT@dict, /) -> _VT@dict
info:   (self, key: _KT@dict, default: _T@get, /) -> _VT@dict | _T@get
info: rule `invalid-argument-type` is enabled by default
```

After reading https://github.com/astral-sh/ty/issues/456 and its implementation https://github.com/astral-sh/ruff/pull/20256, this seems to be working as designed? Top materialization means we can't call the `.get` method here since there is no type for the key would satisfy all possible dicts of any key type, thus the `Never` type. The ecosystem analysis result on https://github.com/astral-sh/ruff/pull/20256 also shows a few examples like the `theme_config` variable [here](https://github.com/mkdocs/mkdocs/blob/2862536793b3c67d9d83c33e0dd6d50a791928f8/mkdocs/config/config_options.py#L827).

However, I couldn't wrap my head around this behavior. Is there a good or pedantic example that `ty` is catching real errors? And at least, could we add better diagnostics or documentation?

(I also found the relevant open issue https://github.com/astral-sh/ty/issues/1130, but it's talking specifically about `TypedDict`.)

### Version

ty 0.0.9 (f1652f05d 2026-01-05)

---

_Comment by @carljm on 2026-01-07 17:11_

Thanks for the report, this is really useful!

There are lots of examples where narrowing generics by top materialization instead of just inserting Unknown specialization catches real errors; the behavior of other type checkers here introduces `Unknown` into fully-typed code and causes lots of unsoundness. Trivial example:

```py
def f(x: object):
    if isinstance(x, list):
        list[0].foobar()
```

Other type checkers allow the above, but it's obviously unsafe, and it's not even clear how you could annotate to get type safe checking here. ty is safe by default on this code.

But your specific case with `dict.get` is more subtle. Your code is perfectly safe, because it's not a type error to pass any random type to `.get()` of any dictionary -- the worst case scenario is that it just returns `None` (or the default). Which means that it's kind of wrong for typeshed to [require `KT` (dictionary key type) as the annotated type of the first argument to `get`](https://github.com/python/typeshed/blob/main/stdlib/builtins.pyi#L1222-L1227); arguably that should be annotated as `object` instead.

But probably people like getting errors if they accidentally pass a string to `.get()` on a `dict[int, ...]`, even though that's not an error at runtime. That's more of a lint rule than a type error, IMO, but nonetheless there would probably be resistance to changing this annotation in typeshed. Which means it's unclear how we should address this situation in ty. At the very least we should improve/add documentation around this, and improve the diagnostic. I think maybe we should even special-case our understanding of `.get()` on the top materialization of `dict` to avoid this error?

---

_Comment by @carljm on 2026-01-07 17:15_

We have https://github.com/astral-sh/ty/issues/2221 already as a broad issue for improving ty's diagnostics in these top-materialization cases. But I'm inclined to leave this issue open specifically targeted to `dict.get` behavior.

---

_Comment by @AlexWaygood on 2026-01-07 17:16_

There have been *quite* a few discussions about the proper signature for `dict.get` on the typeshed issue tracker FWIW — even among mypy/pyright users, I'd say this is probably one of the top 10 most controversial annotations in typeshed. So changing it wouldn't necessarily be easy, but it also wouldn't necessarily be something that would only benefit ty users

---

_Renamed from "`isinstance(..., dict)` and top materialization result in unintuitive errors" to "`isinstance(..., dict)` and top materialization result in false positive on `.get()` call" by @carljm on 2026-01-07 18:15_

---

_Added to milestone `Stable` by @carljm on 2026-01-07 18:15_

---
