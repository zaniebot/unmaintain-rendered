```yaml
number: 845
title: "Implement `@warnings.deprecated` supression for uses of imported symbols"
type: issue
state: open
author: Gankra
labels:
  - feature
  - diagnostics
assignees: []
created_at: 2025-07-17T16:59:24Z
updated_at: 2025-11-14T09:13:24Z
url: https://github.com/astral-sh/ty/issues/845
synced_at: 2026-01-10T02:06:24Z
```

# Implement `@warnings.deprecated` supression for uses of imported symbols

---

_Issue opened by @Gankra on 2025-07-17 16:59_

I'm landing https://github.com/astral-sh/ruff/pull/19376 without this functionality.

---

If you `from module import something_deprecated` we should emit a diagnostic for that **but then not emit diagnostics for subsequent uses of something_deprecated in the file** to minimize needless repetition when there's a single root issue.

---

See this section of the deprecated mdtest:

https://github.com/astral-sh/ruff/blob/8ded41b7e5d9cb3c7883bdb6cdd4acbda2512305/crates/ty_python_semantic/resources/mdtest/deprecated.md#direct-import-deprecated

----

I think this is probably not too hard to do, although it depends on how "right" you want to do it. Notably ideally we should have this behaviour:

```
import module
from module import some_deprecated  # error: [deprecated]

some_deprecated()  # ok! (supressed)
module.some_deprecated()  # error: [deprecated]
```

However a potentially simple implementation of this behaviour would be to, when we emit a diagnostic while checking imports, record the `DeprecatedInstance` as "handled" in the scope and supress all subsequent diagnostics that involve that `DeprecatedInstance` (or maybe mark the FunctionLiteral/ClassLiteral? unclear if there's relevant corner cases to that difference).

That implementation would supress `module.some_deprecated()` until the `from module import some_deprecated` was handled, at which point it would suddenly become unsupressed and complain. While this is certainly suboptimal I think this is so narrow that the "flaw" is acceptable.

Of course if the "correct" implementation is easy, great. I have no idea how to do the correct one in ty's architecture though.

---

_Label `feature` added by @Gankra on 2025-07-17 16:59_

---

_Label `diagnostics` added by @Gankra on 2025-07-17 16:59_

---

_Comment by @carljm on 2025-07-18 22:17_

Another possible implementation approach here would be that when we see an import or assignment of a deprecated function/class, after raising the deprecation diagnostic, we actually assign/import a modified version of the type with the `deprecated` field cleared to `None`.

This would probably require that we also implement `normalized_impl` on functions/classes such that they always normalize to "non-deprecated", so that we still consider the deprecated and non-deprecated literal types for the same function/class as equivalent types. This is a bit ugly, but may still be overall nicer/simpler than other ways of implementing this suppression.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 09:13_

---
