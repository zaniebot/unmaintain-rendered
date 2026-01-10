```yaml
number: 1189
title: "[panic] infer_definition_types(Id(22f5c)): execute: too many cycle iterations"
type: issue
state: closed
author: mawildoer
labels: []
assignees: []
created_at: 2025-09-16T04:17:46Z
updated_at: 2025-09-16T08:28:20Z
url: https://github.com/astral-sh/ty/issues/1189
synced_at: 2026-01-10T02:06:25Z
```

# [panic] infer_definition_types(Id(22f5c)): execute: too many cycle iterations

---

_Issue opened by @mawildoer on 2025-09-16 04:17_

info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.20 (f41f00af1 2025-09-03)
info: Args: ["ty", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(23835))
             at crates/ty_python_semantic/src/types/infer.rs:174
   1: place_by_id(Id(c04b))
             at crates/ty_python_semantic/src/place.rs:702
   2: Type < 'db >::member_lookup_with_policy_(Id(b859))
             at crates/ty_python_semantic/src/types.rs:715
   3: infer_definition_types(Id(20861))
             at crates/ty_python_semantic/src/types/infer.rs:174
   4: place_by_id(Id(c04a))
             at crates/ty_python_semantic/src/place.rs:702
   5: Type < 'db >::member_lookup_with_policy_(Id(b858))
             at crates/ty_python_semantic/src/types.rs:715
   6: infer_definition_types(Id(3832))
             at crates/ty_python_semantic/src/types/infer.rs:174
   7: infer_scope_types(Id(2804))
             at crates/ty_python_semantic/src/types/infer.rs:145
   8: check_file_impl(Id(c02))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

---

_Comment by @sharkdp on 2025-09-16 08:28_

Thank you for reporting this. It is very likely that this is another instance of #256. I'm going to close this as a duplicate, but feel free to comment here with an MRE if you think it's unrelated.

---

_Closed by @sharkdp on 2025-09-16 08:28_

---
