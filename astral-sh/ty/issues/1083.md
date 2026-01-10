```yaml
number: 1083
title: "[panic] on infer_definition_types"
type: issue
state: closed
author: rgimen3z
labels: []
assignees: []
created_at: 2025-08-21T17:50:42Z
updated_at: 2025-08-21T17:53:31Z
url: https://github.com/astral-sh/ty/issues/1083
synced_at: 2026-01-10T02:06:24Z
```

# [panic] on infer_definition_types

---

_Issue opened by @rgimen3z on 2025-08-21 17:50_

Opening an issue, as requested by `ty`:

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `<MY_PATH_HERE>`: `infer_definition_types(Id(5a22c)): execute: too many cycle iterations`
```

```
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.19 (e9cb838b3 2025-08-19)
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_expression_types(Id(6a0a4))
             at crates/ty_python_semantic/src/types/infer.rs:248
   1: infer_definition_types(Id(69d19))
             at crates/ty_python_semantic/src/types/infer.rs:167
   2: place_by_id(Id(33464))
             at crates/ty_python_semantic/src/place.rs:688
   3: Type < 'db >::member_lookup_with_policy_(Id(18495))
             at crates/ty_python_semantic/src/types.rs:635
   4: infer_definition_types(Id(62fcc))
             at crates/ty_python_semantic/src/types/infer.rs:167
   5: Type < 'db >::class_member_with_policy_(Id(3a4e7))
             at crates/ty_python_semantic/src/types.rs:635
   6: Type < 'db >::member_lookup_with_policy_(Id(139a4))
             at crates/ty_python_semantic/src/types.rs:635
   7: infer_scope_types(Id(5a59a))
             at crates/ty_python_semantic/src/types/infer.rs:138
   8: check_file_impl(Id(c25))
             at crates/ty_project/src/lib.rs:527
```

---

_Comment by @carljm on 2025-08-21 17:51_

Thanks!

---

_Closed by @carljm on 2025-08-21 17:51_

---
