```yaml
number: 20286
title: "[ty] Fall back to `object` for attribute access on synthesized protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/synthetic-proto-attributes
created_at: 2025-09-07T17:08:51Z
updated_at: 2025-09-08T12:04:38Z
url: https://github.com/astral-sh/ruff/pull/20286
synced_at: 2026-01-10T17:46:22Z
```

# [ty] Fall back to `object` for attribute access on synthesized protocols

---

_Pull request opened by @AlexWaygood on 2025-09-07 17:08_

## Summary

Currently we have false positives for things like this:

```py
def f(x: object):
    if hasattr(x, "__qualname__"):
        reveal_type(x.__dict__)
        reveal_type(x.__repr__)
        reveal_type(x.__reduce__)
```

We complain that none of these attributes exist on an object of type `<Protocol with members '__qualname__'>`. But these attributes all exist on typeshed's stub for `object`, so these complaints don't make much sense.

It looks like the issue is that `ClassLiteral::own_instance_member()` returns `Place::Unbound` for attributes that are declared in the class body but are not bound in any instance methods:

https://github.com/astral-sh/ruff/blob/2467c4352e0f456e16a068ab5b7d09da009df8e2/crates/ty_python_semantic/src/types/class.rs#L3176-L3204

So this PR switches the attribute-fallback logic to use `Type::object(db).member(...)` rather than `Type::object(db).instance_member(db)`.

~~I'm not totally sure if this is the correct fix or not, to be honest! It seems like it also leads to some regressions: we no longer understand that these attributes are read-only, and I'm not totally sure why.~~ With @sharkdp's help, I fixed the regressions.

## Test Plan

Mdtests added


---

_Label `ty` added by @AlexWaygood on 2025-09-07 17:08_

---

_Comment by @github-actions[bot] on 2025-09-07 17:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-07 17:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-09-07 17:25_

Hmm, okay, this definitely isn't quite right... on this branch, we no longer emit _any_ diagnostics for unresolved attributes on synthesized protocols:

```py
def f(x: object):
    if hasattr(x, '__qualname__'):
        reveal_type(x)                 # `<Protocol with members '__qualname__'>`
        reveal_type(x.__qualname__)    # `object` (good!)
        reveal_type(x.__module__)      # `str` (good!)
        reveal_type(x.__foo__)         # `Unknown` (good) but no diagnostic (??)
```

---

_Comment by @AlexWaygood on 2025-09-07 17:28_

@sharkdp, I might need your help figuring this one out...

---

_Comment by @sharkdp on 2025-09-08 07:47_

> @sharkdp, I might need your help figuring this one out...

The problem is `object.__getattribute__`. When you access an unknown member (`__foo__`) on `<Protocol with members '__qualname__'>`, `member_lookup_with_policy` will try to fall back to a `__getattribute__` method on that type. It will do so by looking up `__getattribute__` with the `MRO_NO_OBJECT_FALLBACK` policy, because we normally want to skip `object.__getattribute__`. But with your change here, you explicitly call `member` without that policy, and so it looks like `__getattribute__` exists on `<Protocol with members '__qualname__'>`.

You can't just pass down that policy here, because otherwise, you would also lose other members of `object`. I guess you could do something like the following, but there are maybe better solutions here. Let me know if I should take another look
```diff
diff --git a/crates/ty_python_semantic/src/types/protocol_class.rs b/crates/ty_python_semantic/src/types/protocol_class.rs
index 6b27905a80..c15b1af9cf 100644
--- a/crates/ty_python_semantic/src/types/protocol_class.rs
+++ b/crates/ty_python_semantic/src/types/protocol_class.rs
@@ -227,7 +227,13 @@ impl<'db> ProtocolInterface<'db> {
                 place: Place::bound(member.ty()),
                 qualifiers: member.qualifiers(),
             })
-            .unwrap_or_else(|| Type::object(db).member(db, name))
+            .unwrap_or_else(|| {
+                if name == "__getattribute__" {
+                    Place::Unbound.into()
+                } else {
+                    Type::object(db).member(db, name)
+                }
+            })
     }
 
     /// Return `true` if if all members on `self` are also members of `other`.
```

---

_Marked ready for review by @AlexWaygood on 2025-09-08 11:50_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-08 11:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-08 11:50_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-08 11:50_

---

_Comment by @AlexWaygood on 2025-09-08 11:51_

> You can't just pass down that policy here, because otherwise, you would also lose other members of `object`. I guess you could do something like the following, but there are maybe better solutions here. Let me know if I should take another look

Thank you very much! I think I found a cleaner, more generalised solution

---

_@sharkdp approved on 2025-09-08 12:01_

Looks good!

---

_Merged by @AlexWaygood on 2025-09-08 12:04_

---

_Closed by @AlexWaygood on 2025-09-08 12:04_

---

_Branch deleted on 2025-09-08 12:04_

---
