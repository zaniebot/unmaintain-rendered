```yaml
number: 1676
title: "Add diagnostic warning against shadowing a submodule name in `__init__.py`"
type: issue
state: open
author: AlexWaygood
labels:
  - imports
assignees: []
created_at: 2025-11-29T15:46:23Z
updated_at: 2025-11-29T15:46:41Z
url: https://github.com/astral-sh/ty/issues/1676
synced_at: 2026-01-10T01:58:59Z
```

# Add diagnostic warning against shadowing a submodule name in `__init__.py`

---

_Issue opened by @AlexWaygood on 2025-11-29 15:46_

If you do something like this, it significantly increases the likelihood that ty will get confused when resolving the type of the attribute `a.b` when `a` has been imported in another module (should it be resolved to `Literal[42]` or `<module 'a.b'>`?:

````md
`a/__init__.py`:

```py
b = 42
```

`a/b.py`:

```py
```
````

I think it would be helpful to our users if we emitted a diagnostic when a symbol is defined in an `__init__.py` file that shadows the name of a submodule. In rare occasions, there might be a good reason for doing this, but in those situations you could easily just disable the warning for that `__init__.py` file in particular.

A common reason for shadowing submodules like this is to actually prohibit users from importing the submodule -- that can be done if you first import the submodule in `__init__.py`, and then override it later on in `__init__.py`. We should therefore consider:
1. Skipping the warning if the submodule has a name beginning with a single underscore (this is the convention to mark a submodule as private)
2. State in the diagnostic message for the warning that the user could consider renaming their submodule so that it starts with a leading underscore

---

_Label `imports` added by @AlexWaygood on 2025-11-29 15:46_

---
