```yaml
number: 1777
title: "Use signature of `__init_subclass__` of superclasses to inform keyword-argument completion suggestions inside class definitions"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T14:55:35Z
updated_at: 2025-12-05T23:05:47Z
url: https://github.com/astral-sh/ty/issues/1777
synced_at: 2026-01-10T01:56:41Z
```

# Use signature of `__init_subclass__` of superclasses to inform keyword-argument completion suggestions inside class definitions

---

_Issue opened by @AlexWaygood on 2025-12-05 14:55_

In this snippet, the class definition `Bar` will fail because `Foo.__init_subclass__` states that subclasses must provide the `special_keyword` keyword argument when you inherit from `Foo`:

```py
class Foo:
    def __init_subclass__(cls, special_keyword):
        pass

class Bar(Foo): ...
```

The definition of `Bar` must instead be:

```py
class Bar(Foo, special_keyword=42): ...
```

or similar. We could look at the `__init_subclass__` signatures of classes already in the class definition to inform autocomplete suggestions for keyword arguments in this context.

---

_Label `server` added by @AlexWaygood on 2025-12-05 14:55_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 14:55_

---

_Comment by @carljm on 2025-12-05 15:58_

It seems like the type checker should also error on the definition of `Bar` that doesn't provide `special_keyword`? I guess that should be a separate issue? (Maybe you already filed one and I didn't see it yet.)

---

_Comment by @AlexWaygood on 2025-12-05 16:03_

yeah I filed that one a few weeks/months ago ;)

---

_Comment by @AlexWaygood on 2025-12-05 16:04_

see https://github.com/astral-sh/ty/issues/1541

---

_Comment by @dangotbanned on 2025-12-05 23:05_

Sorely missing this in `pyright`/`pylance`, would be great to see better support in `ty`

- https://github.com/microsoft/pylance-release/discussions/2781

(I assume these are related, but ignore me if not)

The lacking `__init_subclass__` support in `pylance` *also* means symbols that are used in the defining context don't show up in `Find All References` either 

---
