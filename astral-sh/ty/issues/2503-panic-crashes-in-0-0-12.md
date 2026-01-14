```yaml
number: 2503
title: Panic crashes in 0.0.12
type: issue
state: open
author: dabao12321
labels:
  - needs-info
  - fatal
assignees: []
created_at: 2026-01-14T23:25:30Z
updated_at: 2026-01-14T23:33:23Z
url: https://github.com/astral-sh/ty/issues/2503
synced_at: 2026-01-14T23:42:12Z
```

# Panic crashes in 0.0.12

---

_@dabao12321_

### Summary

After updating to 0.0.12, we've seen consistent panic crashes on multiple files that didn't happen before with 0.0.11 with e.g. the following errors:

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:9021:14 when checking `/Users/amandali/Documents/work/solomon/libs/servicelib/servicelib/template_resolver/data_catalog.py`: `assertion `left == right` failed
  left: Some(StringLiteral(StringLiteralType { value: "label" }))
 right: None`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.12 (4b74e4ded 2026-01-14)
info: Args: ["ty", "check", "libs/servicelib/servicelib/template_resolver/data_catalog.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(7fc4))
             at crates/ty_python_semantic/src/types/infer.rs:104
   1: place_by_id(Id(380d))
             at crates/ty_python_semantic/src/place.rs:877
   2: Type < 'db >::member_lookup_with_policy_(Id(3409))
             at crates/ty_python_semantic/src/types.rs:873
   3: infer_definition_types(Id(1404))
             at crates/ty_python_semantic/src/types/infer.rs:104
   4: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
   5: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:554


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

for code:
```
"label": {
                "label": "Label",
                "type": "property",
                "access_path": "label",
            },
```

### Version

0.0.12

---

_Label `fatal` added by @carljm on 2026-01-14 23:28_

---

_Label `needs-info` added by @carljm on 2026-01-14 23:28_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-14 23:28_

---

_Comment by @carljm on 2026-01-14 23:33_

Thanks for the report! Is there any chance you can provide any more code context? The given snippet doesn't seem to cause ty any trouble. Are you assigning this nested dict to a variable typed as a TypedDict, maybe?

This looks like something which I'd expect to reproduce consistently given the same code. Does it? If so, is it possible you could minimize a code snippet which causes it to happen?

Or alternatively, running with `RUST_BACKTRACE=1` and providing a full stack trace could also help us track it down, but this would probably require building a debug build of ty for the stack trace to have useful information.

---
