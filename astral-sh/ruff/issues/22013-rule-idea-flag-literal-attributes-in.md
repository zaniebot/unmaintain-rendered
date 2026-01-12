```yaml
number: 22013
title: "Rule idea: flag literal attributes in monkeypatching/mocking APIs"
type: issue
state: open
author: woodruffw
labels:
  - rule
assignees: []
created_at: 2025-12-16T20:49:00Z
updated_at: 2025-12-16T22:13:57Z
url: https://github.com/astral-sh/ruff/issues/22013
synced_at: 2026-01-12T15:54:58Z
```

# Rule idea: flag literal attributes in monkeypatching/mocking APIs

---

_@woodruffw_

### Summary

This is a rough idea that probably needs refining/discussion 🙂 

## Overview

Pytest and others expose monkeypatch/mocking APIs like:

```python
monkeypatch.setattr(some_module, "my_important_api", replacement)
```

(and same for `delattr`.)

When called, this temporarily monkeypatches `some_module.my_important_api` with `replacement` (which could be an object, callable, etc.).

## Problem statement

The above works, but has a footgun: if the user performs a refactor (e.g. from `my_important_api` to `my_critical_api`), then may miss this monkeypatch (since it's a string literal, not a usage/reference). This is particularly likely to happen with IDE/LSP refactors, since syntax-aware rename-all functionality can't look at these kinds of non-type string literals by design.

The impact of this depends: sometimes users notice it immediately (since their monkeypatch stops working), while other times it makes tests subtly incorrect or unreliable in ways that aren't noticed until much later.

## Solution statement

In general, users should _probably_ write:

```python
monkeypatch.setattr(some_module, some_module.my_important_api.__name__, replacement)
```

That'll behave the same as above, _but_ will ensure that refactors/mechanical renames propagate as expected.

## Problems

1. This won't work with dynamic attributes; not sure if this is a problem or not.
2. Using `__name__` is arguably pretty unsightly and is maybe considered unidiomatic, not sure 🙂 

## Alternatives

1. Do nothing.
2. Maybe Pytest and others with monkeypatching APIs should support functions as first-class "nameable" objects? In other words, Ruff could instead recommend:

    ```python
    monkeypatch.setattr(some_module, some_module.my_important_api.__name__, replacement)
    ```
    
    ...and Pytest would do the `__name__` access internally. This would require upstream changes, but would be a lot visually cleaner from the user's side.

## Other references

xref https://github.com/astral-sh/ruff/issues/12850 for another (but I think unrelated) mocking/monkeypatching feature request.

---

_Comment by @carljm on 2025-12-16 20:49_

This seems like something that needs to be cross-module and have type information.

---

_Label `type-inference` added by @carljm on 2025-12-16 20:50_

---

_Comment by @ntBre on 2025-12-16 21:18_

If I understand correctly, we could get by without type inference if we're just looking for `monkeypatch.setattr` with a string as the second argument and recommending to use `__name__` instead. I think we'd only need type inference if we were trying to resolve the definition of the string. But I could definitely be missing something.

---

_Label `rule` added by @ntBre on 2025-12-16 21:18_

---

_Comment by @carljm on 2025-12-16 21:20_

Oh, yeah, I misunderstood the proposal. A type-aware lint rule could actually tell you when your patch is patching something that doesn't exist, but a non-type-aware rule could just tell you to not use strings.

---

_Label `type-inference` removed by @carljm on 2025-12-16 21:20_

---

_Comment by @woodruffw on 2025-12-16 22:13_

Yeah, sorry for the confusion! I was thinking of this as a purely naive lint, i.e. no type or actual attribute awareness.

---
