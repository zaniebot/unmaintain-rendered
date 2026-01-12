```yaml
number: 843
title: "Implement `@warnings.deprecated` checking for dunder methods and properties"
type: issue
state: open
author: Gankra
labels:
  - feature
  - diagnostics
assignees: []
created_at: 2025-07-17T16:41:22Z
updated_at: 2025-11-13T17:56:04Z
url: https://github.com/astral-sh/ty/issues/843
synced_at: 2026-01-12T15:54:24Z
```

# Implement `@warnings.deprecated` checking for dunder methods and properties

---

_@Gankra_

I'm landing https://github.com/astral-sh/ruff/pull/19376 without this functionality.

---

Per https://typing.python.org/en/latest/spec/directives.html#deprecated

> Any syntax that indirectly triggers a call to the function. For example, if the __add__ method of a class C is deprecated, then the code C() + C() should trigger a diagnostic. Similarly, if the setter of a property is marked deprecated, attempts to set the property should trigger a diagnostic.

----

See this section of the mdtest for a TODO test of the `__add__` dunder:

https://github.com/astral-sh/ruff/blob/8ded41b7e5d9cb3c7883bdb6cdd4acbda2512305/crates/ty_python_semantic/resources/mdtest/deprecated.md#dunders

----

I have no idea if this is really easy or complicated.



---

_Label `feature` added by @Gankra on 2025-07-17 16:41_

---

_Label `diagnostics` added by @Gankra on 2025-07-17 16:41_

---

_Added to milestone `Beta` by @MichaReiser on 2025-07-21 14:55_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-07-21 14:55_

---

_Added to milestone `GA` by @MichaReiser on 2025-07-21 14:55_

---

_Assigned to @Gankra by @Gankra on 2025-11-13 17:56_

---
