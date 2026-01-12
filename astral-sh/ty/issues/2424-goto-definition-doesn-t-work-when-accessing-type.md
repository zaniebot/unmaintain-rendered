```yaml
number: 2424
title: "Goto-definition doesn't work when accessing `type` attributes on class objects"
type: issue
state: open
author: AlexWaygood
labels:
  - server
assignees: []
created_at: 2026-01-09T19:52:28Z
updated_at: 2026-01-09T19:52:28Z
url: https://github.com/astral-sh/ty/issues/2424
synced_at: 2026-01-12T15:54:26Z
```

# Goto-definition doesn't work when accessing `type` attributes on class objects

---

_@AlexWaygood_

For this snippet:

```py
class Foo: ...

Foo.__dictoffset__
```

Ty correctly infers that the type of the `__dictoffset__` attribute is an `int`. However, goto-definition doesn't take me to the definition of this attribute (which is found at https://github.com/astral-sh/ruff/blob/a0f2cd0ded976511b1746c56d289c0cdb2320098/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L261-L262 -- all class objects are instances of `type`, so all attributes defined on `builtins.type` are available on a class object).

---

_Label `server` added by @AlexWaygood on 2026-01-09 19:52_

---
