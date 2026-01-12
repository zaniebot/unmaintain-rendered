```yaml
number: 16290
title: "[red-knot] Restrict visibility of the `module_type_symbols` function"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/inline-salsa-query
created_at: 2025-02-20T20:39:58Z
updated_at: 2025-02-21T10:55:23Z
url: https://github.com/astral-sh/ruff/pull/16290
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Restrict visibility of the `module_type_symbols` function

---

_@AlexWaygood_

(This PR is stacked on top of https://github.com/astral-sh/ruff/pull/16284; review that first.)

## Summary

This function here is solely an implementation detail of `module_type_symbol()` (which #16284 renames to `module_type_implicit_global_symbol()`):

https://github.com/astral-sh/ruff/blob/470f852f04f0e4284c78d15ce81637540e127558/crates/red_knot_python_semantic/src/symbol.rs#L661-L694

In fact, not only is it an implementation detail -- it would be _wrong_ for any other function in `symbol.rs` to use it. For example, this function also falls back to `types.ModuleType`, but in a slightly different way so that only `__getattr__` is excluded from the declared types on `types.ModuleType`. The fact that the other function does the fallback differently is deliberate, because the other function implements lookup in the global scope _from external-module scopes_ whereas the callers of `module_type_implicit_global_symbol()` are implementing lookup in the global scope _from scopes in the same file_.

This PR restricts the visibility of the `module_type_symbols()` by moving it into an inner module, so that only `module_type_implicit_global_symbol()` can use it. It also further improves the documentation so that it's clear how it differs from the other `ModuleType` fallback implemented in `symbol.rs`.

My first idea here was to make `module_type_symbols()` an inner function nested inside `module_type_implicit_global_symbol()`. However, that would have required removing the unit test for `module_type_symbols`.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-20 20:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-20 20:39_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-20 20:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-20 20:39_

---

_@carljm approved on 2025-02-20 21:33_

Looks good!

Not necessarily for this PR, but if it was such a performance win to have `module_type_implicit_global_symbol` with its cached check of the names, it makes me wonder whether it would be a win to do something similar for the fallback in `imported_symbol`.

And I just think the subtle distinction between the fallback in `imported_symbol` and the one using `module_type_implicit_global_symbol` might be clearer if the structure was more similar, with parallel functions.

---

_Comment by @AlexWaygood on 2025-02-21 10:54_

> Not necessarily for this PR, but if it was such a performance win to have `module_type_implicit_global_symbol` with its cached check of the names, it makes me wonder whether it would be a win to do something similar for the fallback in `imported_symbol`.
> 
> And I just think the subtle distinction between the fallback in `imported_symbol` and the one using `module_type_implicit_global_symbol` might be clearer if the structure was more similar, with parallel functions.

It's a tempting idea, but unfortunately it would be incorrect if we just did in the same way as with `module_type_implicit_global_symbol`. When we do the global-scope lookup from external modules, we need to model the fact that attributes on supertypes of `types.ModuleType` are also available, as well as attributes on `types.ModuleType` itself. E.g.:

```pycon
>>> from typing import __repr__
>>> __repr__
<method-wrapper '__repr__' of module object at 0x103cae660>
```

So we cannot simply short-circuit if the name is not found in the list of symbols found in the `types.ModuleType` class body.

Now, we _could_ instead short-circuit if the name is not found in _either_ the list of symbols found in the `types.ModuleType` class body _or_ the `builtins.object` class body (since we know that `types.ModuleType` just inherits directly from `object`. I suppose that might work, and probably wouldn't be too complicated!

---

_Merged by @AlexWaygood on 2025-02-21 10:55_

---

_Closed by @AlexWaygood on 2025-02-21 10:55_

---

_Branch deleted on 2025-02-21 10:55_

---
