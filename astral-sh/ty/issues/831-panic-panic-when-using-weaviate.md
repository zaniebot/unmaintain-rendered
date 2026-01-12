```yaml
number: 831
title: "[panic] Panic when using weaviate"
type: issue
state: closed
author: bartaugust
labels: []
assignees: []
created_at: 2025-07-16T14:59:37Z
updated_at: 2025-07-16T15:02:11Z
url: https://github.com/astral-sh/ty/issues/831
synced_at: 2026-01-12T15:54:24Z
```

# [panic] Panic when using weaviate

---

_@bartaugust_

Using 
from weaviate.collections import Collection
from weaviate.collections.classes.grpc import MetadataQuery


  response = collection.query.near_text(
      query=query,
      limit=limit,
      return_metadata=MetadataQuery(distance=True)
  ) 



error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `{path_to_file`: `infer_definition_types(Id(34d9f)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(13062))
             at crates/ty_python_semantic/src/place.rs:638
   1: Type < 'db >::member_lookup_with_policy_(Id(110f5))
             at crates/ty_python_semantic/src/types.rs:590
   2: infer_definition_types(Id(2d5d4))
             at crates/ty_python_semantic/src/types/infer.rs:159
   3: infer_definition_types(Id(2d646))
             at crates/ty_python_semantic/src/types/infer.rs:159
   4: place_by_id(Id(13059))
             at crates/ty_python_semantic/src/place.rs:638
   5: Type < 'db >::class_member_with_policy_(Id(3f474))
             at crates/ty_python_semantic/src/types.rs:590
   6: Type < 'db >::member_lookup_with_policy_(Id(11058))
             at crates/ty_python_semantic/src/types.rs:590
   7: infer_expression_types(Id(9ab2))
             at crates/ty_python_semantic/src/types/infer.rs:235
   8: infer_definition_types(Id(2d5b0))
             at crates/ty_python_semantic/src/types/infer.rs:159
   9: infer_scope_types(Id(7c8c))
             at crates/ty_python_semantic/src/types/infer.rs:130
  10: check_types(Id(c20))
             at crates/ty_python_semantic/src/types.rs:91


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

---

_Comment by @AlexWaygood on 2025-07-16 15:02_

Thank you. This is a duplicate of #256 (see my explanation in #683)

---

_Closed by @AlexWaygood on 2025-07-16 15:02_

---
