```yaml
number: 790
title: Incorrect implementation of equivalence and subtyping for module-literal types?
type: issue
state: closed
author: AlexWaygood
labels:
  - type properties
assignees: []
created_at: 2025-07-09T10:49:27Z
updated_at: 2025-07-10T09:11:12Z
url: https://github.com/astral-sh/ty/issues/790
synced_at: 2026-01-12T15:54:24Z
```

# Incorrect implementation of equivalence and subtyping for module-literal types?

---

_@AlexWaygood_

### Summary

Consider this situation:

`module.py`:

```py
import typing
```

`main.py`:

```py
from ty_extensions import is_subtype_of, is_equivalent_to, TypeOf

import typing
from module import typing as other_typing

reveal_type(is_subtype_of(TypeOf[typing], TypeOf[other_typing]))
reveal_type(is_equivalent_to(TypeOf[typing], TypeOf[other_typing]))
```

https://play.ty.dev/251c9d98-2f71-4b0d-a627-8d2c4f40770b

The `reveal_type` calls both reveal `Literal[False]` here, even though `typing` refers to the same module as `other_typing`. This is because our implementation of equivalence and subtyping for module-literal types just does a naive `==` comparison between two `ModuleLiteralType`s to determine whether one `ModuleLiteralType` is equivalent to another. But these two "copies" of the `typing` module in our representation were originally imported by different modules, so they do not compare equal, because we record the importing module on the type itself in the `importing_file` field on `ModuleLiteralType`: https://github.com/astral-sh/ruff/blob/f32f7a3b48ff11351c709c60109fafd5ca18ec6c/crates/ty_python_semantic/src/types.rs#L7499-L7510

The reason _why_ we store the `importing_file` field on `ModuleLiteralType`s at all is so that we can check whether any submodules of that module have been imported in the same file that imported the module. If there were, then we recognize those submodules as being available as attributes on the `ModuleLiteralType`. That means that for something like this, we would recognize an `abc` attribute as being available on `other_importlib` but not `importlib`:


`module.py`:

```py
import importlib
import importlib.abc
```

`main.py`:

```py
import importlib
from module import importlib as other_importlib

# error: Type `<module 'importlib'>` has no attribute `abc` (unresolved-attribute)
reveal_type(importlib.abc)  # revealed: Unknown

reveal_type(other_importlib.abc)  # revealed: <module 'importlib.abc'>
```

https://play.ty.dev/8b92c614-403a-47be-84dc-f3e3321f62c1

So perhaps there _is_ an argument that our current implementation is actually correct -- we _could_ treat the two `ModuleLiteralType`s differently in some situations, so they shouldn't be seen as equivalent types, even if they point to the same underlying module?

In any case, we should add some tests for this, and some comments to the test explaining why this is currently the case -- I don't think we have any at the moment. No tests fail if I apply this diff to `main`, which changes the result of the `reveal_type` calls in my original snippet from `Literal[False]` to `Literal[True]`

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 71e7ab5798..1b3fe9ab5d 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -1355,6 +1355,9 @@ impl<'db> Type<'db> {
             (Type::MethodWrapper(self_method), Type::MethodWrapper(target_method)) => {
                 self_method.has_relation_to(db, target_method, relation)
             }
+            (Type::ModuleLiteral(self_module), Type::ModuleLiteral(target_module)) => {
+                self_module.module(db).file() == target_module.module(db).file()
+            }
 
             // No literal type is a subtype of any other literal type, unless they are the same
             // type (which is handled above). This case is not necessary from a correctness
@@ -1630,6 +1633,9 @@ impl<'db> Type<'db> {
             | (nominal @ Type::NominalInstance(n), Type::ProtocolInstance(protocol)) => {
                 n.class.is_object(db) && protocol.normalized(db) == nominal
             }
+            (Type::ModuleLiteral(first), Type::ModuleLiteral(second)) => {
+                first.module(db).file() == second.module(db).file()
+            }
             _ => false,
         }
     }
```

---

_Label `type properties` added by @AlexWaygood on 2025-07-09 10:49_

---

_Renamed from "Incorrect implementation of equivalence and subtyping for module-literal types" to "Incorrect implementation of equivalence and subtyping for module-literal types?" by @AlexWaygood on 2025-07-09 10:49_

---

_Closed by @AlexWaygood on 2025-07-10 09:11_

---
