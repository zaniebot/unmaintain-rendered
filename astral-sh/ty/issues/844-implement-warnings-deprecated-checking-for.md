```yaml
number: 844
title: "Implement `@warnings.deprecated` checking for `@override`s"
type: issue
state: open
author: Gankra
labels:
  - feature
  - diagnostics
assignees: []
created_at: 2025-07-17T16:43:29Z
updated_at: 2025-11-13T17:56:15Z
url: https://github.com/astral-sh/ty/issues/844
synced_at: 2026-01-10T02:06:24Z
```

# Implement `@warnings.deprecated` checking for `@override`s

---

_Issue opened by @Gankra on 2025-07-17 16:43_

I'm landing https://github.com/astral-sh/ruff/pull/19376 without this functionality.

---

Per https://typing.python.org/en/latest/spec/directives.html#deprecated

> If a method is marked with the [@override decorator](https://typing.python.org/en/latest/spec/class-compat.html#override) and the base class method it overrides is deprecated, the type checker should produce a diagnostic.

----

The deprecated mdtest does not currently stress this situation. I have no idea how hard this is to do.



---

_Label `feature` added by @Gankra on 2025-07-17 16:43_

---

_Label `diagnostics` added by @Gankra on 2025-07-17 16:43_

---

_Comment by @jelle-openai on 2025-07-17 18:58_

ty doesn't appear to support `@override` at all yet (#155), so this doesn't make much sense to do yet. This feature should either be part of the implementation of `@override` when that arrives, or be implemented afterwards.

I imagine this is pretty easy to add as part of a general `@override` implementation. If you see an `@override` decorator, look for a corresponding method on a base class. If you don't find such a method, issue a diagnostic. If you find it but it's deprecated, also issue a diagnostic.

---

_Added to milestone `GA` by @MichaReiser on 2025-07-21 14:55_

---

_Assigned to @Gankra by @Gankra on 2025-11-13 17:33_

---

_Unassigned @Gankra by @Gankra on 2025-11-13 17:56_

---
