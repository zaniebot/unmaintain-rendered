```yaml
number: 19102
title: "Filter private `builtins` members from completions"
type: pull_request
state: closed
author: zanieb
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: zb/filter-builtin-under
created_at: 2025-07-02T20:30:46Z
updated_at: 2025-07-03T16:28:42Z
url: https://github.com/astral-sh/ruff/pull/19102
synced_at: 2026-01-10T18:33:12Z
```

# Filter private `builtins` members from completions

---

_Pull request opened by @zanieb on 2025-07-02 20:30_

Closes https://github.com/astral-sh/ty/issues/757

A few thoughts here

1. This doesn't explicitly filter from the typeshed builtins as discussed in the issue, is that equivalent to the builtins module for our purposes or do we need to distinguish between them?
2. ~This doesn't distinguish between sunder and dunder, but probably should.~ There are dunder methods in the builtins stubs, but not at the top-level? It doesn't seem like the completions distinguish between these concepts though, as it'll suggest me `__annotations__` in the test? I added a test case around this for clarity. It'd be trivial to retain dunder completions, we'd just need to decide if we want to pull the nice `Kind` sorter out of the classification code for re-use. **N.B.**: See [thread](https://github.com/astral-sh/ruff/pull/19102#discussion_r2180951944) — I changed this in https://github.com/astral-sh/ruff/commit/fc3b1ed523e319c7d291b0c35aea804cf4472453.

And as pointed out by @AlexWaygood 

3. This is really a problem for all stub files, not just the builtins
4. This should probably just filter names that wouldn't exist at runtime, which would be "all private typevars/ParamSpecs/typevartuples in stubs, all private type aliases in stubs, and any classes decorated with @type_check_only" but not necessarily all private names in stubs

There is some context in the internal Discord, [here](https://discord.com/channels/1039017663004942429/1343690517745111081/1381233188114010135) and [here](https://discord.com/channels/1039017663004942429/1343690517745111081/1390079232780271658).

It seems plausible and reasonable to perform (3) and (4) as follow-ups.

---

_Label `server` added by @zanieb on 2025-07-02 20:30_

---

_Label `ty` added by @zanieb on 2025-07-02 20:30_

---

_Label `server` added by @zanieb on 2025-07-02 20:30_

---

_Label `ty` added by @zanieb on 2025-07-02 20:30_

---

_Comment by @github-actions[bot] on 2025-07-02 20:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~45MB
+     memo fields = ~41MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~72MB
+     memo fields = ~66MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~156MB
+     memo fields = ~142MB

discord.py (https://github.com/Rapptz/discord.py)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~251MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

cwltool (https://github.com/common-workflow-language/cwltool)
-     memo fields = ~228MB
+     memo fields = ~207MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB

meson (https://github.com/mesonbuild/meson)
-     memo fields = ~334MB
+     memo fields = ~304MB

```
</details>


---

_@zanieb reviewed on 2025-07-02 20:53_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/semantic_model.rs`:77 on 2025-07-02 20:53_

We could also filter _after_ constructing the `Completion`, or filter _elsewhere_ in the file. I'm not familiar enough to gauge the trade-offs here, but this spot seems reasonable.

---

_@zanieb reviewed on 2025-07-02 20:53_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:476 on 2025-07-02 20:53_

Note I'm guessing we do not want this last assertion.

---

_@zanieb reviewed on 2025-07-02 20:53_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/semantic_model.rs`:77 on 2025-07-02 20:53_

Regarding `_` vs `__`, see https://github.com/astral-sh/ruff/pull/19102/files#r2180951944 and the OP.

---

_@zanieb reviewed on 2025-07-02 22:01_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:476 on 2025-07-02 22:01_

I took the time to do this https://github.com/astral-sh/ruff/pull/19102/commits/fc3b1ed523e319c7d291b0c35aea804cf4472453 as it seems wrong as-is.

---

_Comment by @zanieb on 2025-07-03 12:04_

I'm interested in landing the incremental change given there was significant discussion around the alternative pattern and it'll take a bit longer to do.

@BurntSushi can you give this an early look?

---

_Review requested from @BurntSushi by @zanieb on 2025-07-03 12:04_

---

_Comment by @AlexWaygood on 2025-07-03 13:00_

This is the change you'd need to apply to `main` to do this in a generalized way that fixes the problem for `builtins` but also other stub files too:

```diff
diff --git a/crates/ty_python_semantic/src/types/ide_support.rs b/crates/ty_python_semantic/src/types/ide_support.rs
index 7cf05fa57f..aac637c561 100644
--- a/crates/ty_python_semantic/src/types/ide_support.rs
+++ b/crates/ty_python_semantic/src/types/ide_support.rs
@@ -1,10 +1,10 @@
 use crate::Db;
-use crate::place::{imported_symbol, place_from_bindings, place_from_declarations};
+use crate::place::{Place, imported_symbol, place_from_bindings, place_from_declarations};
 use crate::semantic_index::place::ScopeId;
 use crate::semantic_index::{
     attribute_scopes, global_scope, imported_modules, place_table, semantic_index, use_def_map,
 };
-use crate::types::{ClassBase, ClassLiteral, KnownClass, Type};
+use crate::types::{ClassBase, ClassLiteral, KnownClass, KnownInstanceType, Type};
 use ruff_python_ast::name::Name;
 use rustc_hash::FxHashSet;
 
@@ -144,13 +144,39 @@ impl AllMembers {
                     let Some(symbol_name) = place_table.place_expr(symbol_id).as_name() else {
                         continue;
                     };
-                    if !imported_symbol(db, file, symbol_name, None)
-                        .place
-                        .is_unbound()
+                    let Place::Type(ty, _) = imported_symbol(db, file, symbol_name, None).place
+                    else {
+                        continue;
+                    };
+
+                    // Filter out names in stub files that are almost certain to be private to the stub.
+                    if symbol_name.starts_with('_')
+                        && !symbol_name.starts_with("__")
+                        && file.path(db).extension() == Some("pyi")
                     {
-                        self.members
-                            .insert(place_table.place_expr(symbol_id).expect_name().clone());
+                        match ty {
+                            Type::NominalInstance(instance) => {
+                                if matches!(
+                                    instance.class.known(db),
+                                    Some(
+                                        KnownClass::TypeVar
+                                            | KnownClass::TypeVarTuple
+                                            | KnownClass::ParamSpec
+                                    )
+                                ) {
+                                    continue;
+                                }
+                            }
+                            Type::ClassLiteral(class) if class.is_protocol(db) => continue,
+                            Type::KnownInstance(
+                                KnownInstanceType::TypeVar(_) | KnownInstanceType::TypeAliasType(_),
+                            ) => continue,
+
+                            _ => {}
+                        }
                     }
+                    self.members
+                        .insert(place_table.place_expr(symbol_id).expect_name().clone());
                 }

```

Local testing from running the playground seems to show everything working as expected, and it doesn't result in us doing any more work since we already have the type right there

---

_Comment by @BurntSushi on 2025-07-03 13:08_

I think @AlexWaygood's approach would also include `_IncompleteInputError`, which is the only sunder attribute (on my system) in the standard `builtins` module:

```
>>> [b for b in dir(builtins) if b.startswith('_') and not b.endswith('__')]
['_', '_IncompleteInputError']
```

And I think that's generally consistent with how we do completions elsewhere.

---

_Comment by @zanieb on 2025-07-03 13:16_

I'll pick that up then

---

_Comment by @zanieb on 2025-07-03 16:28_

Replaced by https://github.com/astral-sh/ruff/pull/19121

---

_Closed by @zanieb on 2025-07-03 16:28_

---
