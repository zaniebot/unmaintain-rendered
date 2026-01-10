```yaml
number: 523
title: "Panic if `--typeshed` is set to a subdirectory of a first-party search path"
type: issue
state: open
author: kkpattern
labels:
  - bug
  - configuration
  - fatal
  - imports
assignees: []
created_at: 2025-05-27T11:12:13Z
updated_at: 2025-06-06T16:03:39Z
url: https://github.com/astral-sh/ty/issues/523
synced_at: 2026-01-10T02:34:10Z
```

# Panic if `--typeshed` is set to a subdirectory of a first-party search path

---

_Issue opened by @kkpattern on 2025-05-27 11:12_

## Reproduce step

1. create a file named  `main.py` with the following content:

```python
def add(a: int, b: int) -> int:
    return a + b

add(1, "2")
```

2. Execute the following commands:

```bash
git clone https://github.com/python/typeshed.git
cd typeshed
git checkout ea39b363e02f47debdf583dd88b477e0b0bd546d
cd ..
ty check --typeshed typeshed main.py
```

Output:

```bash
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/function/fetch.rs:143:25 when checking `/home/.../ty-playground/main.py`: `dependency graph cycle when querying try_call_dunder_get_(Id(6802)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(c00)),
    infer_scope_types(Id(1000)),
    explicit_bases_(Id(2c02)),
    infer_deferred_types(Id(32c8)),
    explicit_bases_(Id(2c03)),
    infer_deferred_types(Id(50cc)),
    explicit_bases_(Id(2c04)),
    infer_deferred_types(Id(505e)),
    explicit_bases_(Id(2c05)),
    infer_deferred_types(Id(5056)),
    infer_definition_types(Id(5014)),
    infer_expression_types(Id(24ac)),
    symbol_by_id(Id(2806)),
    infer_expression_type(Id(248b)),
    infer_expression_types(Id(248b)),
    member_lookup_with_policy_(Id(4c01)),
    symbol_by_id(Id(2807)),
    infer_definition_types(Id(550b)),
    explicit_bases_(Id(2c0c)),
    infer_deferred_types(Id(550a)),
    infer_definition_types(Id(5488)),
    explicit_bases_(Id(2c15)),
    infer_deferred_types(Id(6c8b)),
    infer_definition_types(Id(57ca)),
    infer_expression_types(Id(254e)),
    try_call_dunder_get_(Id(6802)),
    class_member_with_policy_(Id(6409)),
    symbol_by_id(Id(280f)),
    infer_definition_types(Id(1453)),
    signature_(Id(4005)),
    infer_deferred_types(Id(5019)),
    infer_definition_types(Id(500d)),
    infer_expression_types(Id(24a5)),
    member_lookup_with_policy_(Id(4c12)),
    try_call_dunder_get_(Id(6801)),
    class_member_with_policy_(Id(6406)),
    try_call_dunder_get_(Id(6804)),
    class_member_with_policy_(Id(6410)),
    class_member_with_policy_(Id(6410)),
    infer_definition_types(Id(3b4f)),
    try_mro_(Id(5805)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "--typeshed", "typeshed", "main.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_expression_types(Id(24a5))
             at crates/ty_python_semantic/src/types/infer.rs:223
             cycle heads: symbol_by_id(Id(2807)) -> 0, symbol_by_id(Id(2806)) -> 0, infer_expression_type(Id(248b)) -> 0, member_lookup_with_policy_(Id(4c01)) -> 0, symbol_by_id(Id(280f)) -> 0, infer_definition_types(Id(1453)) -> 0
   1: infer_definition_types(Id(500d))
             at crates/ty_python_semantic/src/types/infer.rs:149
   2: infer_deferred_types(Id(5019))
             at crates/ty_python_semantic/src/types/infer.rs:187
   3: signature_(Id(4005))
             at crates/ty_python_semantic/src/types.rs:6927
   4: infer_definition_types(Id(1453))
             at crates/ty_python_semantic/src/types/infer.rs:149
             cycle heads: symbol_by_id(Id(2807)) -> 0, symbol_by_id(Id(2806)) -> 0, infer_expression_type(Id(248b)) -> 0, member_lookup_with_policy_(Id(4c01)) -> 0
   5: symbol_by_id(Id(280f))
             at crates/ty_python_semantic/src/symbol.rs:586
   6: class_member_with_policy_(Id(6409))
             at crates/ty_python_semantic/src/types.rs:550
   7: try_call_dunder_get_(Id(6802))
             at crates/ty_python_semantic/src/types.rs:550
   8: infer_expression_types(Id(254e))
             at crates/ty_python_semantic/src/types/infer.rs:223
             cycle heads: symbol_by_id(Id(2806)) -> 0, infer_expression_type(Id(248b)) -> 0, symbol_by_id(Id(2807)) -> 0, member_lookup_with_policy_(Id(4c01)) -> 0
   9: infer_definition_types(Id(57ca))
             at crates/ty_python_semantic/src/types/infer.rs:149
  10: infer_deferred_types(Id(6c8b))
             at crates/ty_python_semantic/src/types/infer.rs:187
  11: explicit_bases_(Id(2c15))
             at crates/ty_python_semantic/src/types/class.rs:551
  12: infer_definition_types(Id(5488))
             at crates/ty_python_semantic/src/types/infer.rs:149
             cycle heads: symbol_by_id(Id(2807)) -> 0, member_lookup_with_policy_(Id(4c01)) -> 0, symbol_by_id(Id(2806)) -> 0, infer_expression_type(Id(248b)) -> 0
  13: infer_deferred_types(Id(550a))
             at crates/ty_python_semantic/src/types/infer.rs:187
  14: explicit_bases_(Id(2c0c))
             at crates/ty_python_semantic/src/types/class.rs:551
  15: infer_definition_types(Id(550b))
             at crates/ty_python_semantic/src/types/infer.rs:149
  16: symbol_by_id(Id(2807))
             at crates/ty_python_semantic/src/symbol.rs:586
  17: member_lookup_with_policy_(Id(4c01))
             at crates/ty_python_semantic/src/types.rs:550
  18: infer_expression_types(Id(248b))
             at crates/ty_python_semantic/src/types/infer.rs:223
  19: infer_expression_type(Id(248b))
             at crates/ty_python_semantic/src/types/infer.rs:279
  20: symbol_by_id(Id(2806))
             at crates/ty_python_semantic/src/symbol.rs:586
  21: infer_expression_types(Id(24ac))
             at crates/ty_python_semantic/src/types/infer.rs:223
  22: infer_definition_types(Id(5014))
             at crates/ty_python_semantic/src/types/infer.rs:149
  23: infer_deferred_types(Id(5056))
             at crates/ty_python_semantic/src/types/infer.rs:187
  24: explicit_bases_(Id(2c05))
             at crates/ty_python_semantic/src/types/class.rs:551
  25: infer_deferred_types(Id(505e))
             at crates/ty_python_semantic/src/types/infer.rs:187
  26: explicit_bases_(Id(2c04))
             at crates/ty_python_semantic/src/types/class.rs:551
  27: infer_deferred_types(Id(50cc))
             at crates/ty_python_semantic/src/types/infer.rs:187
  28: explicit_bases_(Id(2c03))
             at crates/ty_python_semantic/src/types/class.rs:551
  29: infer_deferred_types(Id(32c8))
             at crates/ty_python_semantic/src/types/infer.rs:187
  30: explicit_bases_(Id(2c02))
             at crates/ty_python_semantic/src/types/class.rs:551
  31: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:122
  32: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:84


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

Without the `--typeshed` ty can work correctly:

```bash
error[invalid-argument-type]: Argument to function `add` is incorrect
 --> main.py:4:8
  |
2 |     return a + b
3 |
4 | add(1, "2")
  |        ^^^ Expected `int`, found `Literal["2"]`
  |
info: Function defined here
 --> main.py:1:5
  |
1 | def add(a: int, b: int) -> int:
  |     ^^^         ------ Parameter declared here
2 |     return a + b
  |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

## Version
ty 0.0.1-alpha.7


---

_Label `fatal` added by @AlexWaygood on 2025-05-27 11:15_

---

_Label `bug` added by @AlexWaygood on 2025-05-27 11:15_

---

_Comment by @sharkdp on 2025-05-27 11:41_

Thank you for reporting this.

This seems to be somehow related to the fact that the custom `typeshed` is in the same directory as the file you're checking. If I move it to a different location, I can not reproduce this anymore.

---

_Comment by @AlexWaygood on 2025-05-27 11:48_

We always add `.` as a first-party search path (emulating the behaviour of Python at runtime). My guess is that we're getting pretty confused here by the standard-library search path being a subdirectory of a first-party search path. 

---

_Comment by @kkpattern on 2025-05-27 11:49_

It seems also related to the `typeshed` dir name. I move the `typeshed` dir to `test/typeshed`, then command `ty check --typeshed test/typeshed main.py` still paniced. But if I move the `typeshed` dir to `test/custom-typeshed`, then command `ty check --typeshed test/custom-typeshed main.py` works correctly.

---

_Comment by @AlexWaygood on 2025-05-27 12:02_

yeah, that doesn't surprise me. `typeshed` is a valid module name, and so is `test.typeshed`, so the file `typeshed/stdlib/builtins.pyi` could plausibly be misinterpreted as being a file that should be resolved relative to a `typeshed.stdlib` namespace package in the first-party search path. But `test.custom-typeshed` is not a valid module name, so it's not possible for ty to misinterpret the search path `test/custom-typeshed/stdlib/builtins.pyi` should be resolved relative to. It _has_ to fallback to the standard-library search path.

---

_Label `imports` added by @AlexWaygood on 2025-05-27 12:08_

---

_Comment by @AlexWaygood on 2025-05-27 12:21_

I confirmed that this patch stops us panicking, meaning that the issue is indeed that we're getting confused about which search path standard-library stub files should be resolved relative to:

```diff
diff --git a/crates/ty_python_semantic/src/module_resolver/resolver.rs b/crates/ty_python_semantic/src/module_resolver/resolver.rs
index 49bf534e77..e3743cec8f 100644
--- a/crates/ty_python_semantic/src/module_resolver/resolver.rs
+++ b/crates/ty_python_semantic/src/module_resolver/resolver.rs
@@ -84,6 +84,15 @@ enum SystemOrVendoredPathRef<'a> {
     Vendored(&'a VendoredPath),
 }
 
+impl<'a> SystemOrVendoredPathRef<'a> {
+    fn into_system_path(self) -> Option<&'a SystemPath> {
+        match self {
+            SystemOrVendoredPathRef::System(path) => Some(path),
+            SystemOrVendoredPathRef::Vendored(_) => None,
+        }
+    }
+}
+
 impl std::fmt::Display for SystemOrVendoredPathRef<'_> {
     fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
         match self {
@@ -107,6 +116,13 @@ pub(crate) fn file_to_module(db: &dyn Db, file: File) -> Option<Module> {
     };
 
     let module_name = search_paths(db).find_map(|candidate| {
+        if !candidate.is_standard_library()
+            && path
+                .into_system_path()
+                .is_some_and(|path| path.components().any(|c| c.as_str() == "typeshed"))
+        {
+            return None;
+        }
         let relative_path = match path {
             SystemOrVendoredPathRef::System(path) => candidate.relativize_system_path(path),
             SystemOrVendoredPathRef::Vendored(path) => candidate.relativize_vendored_path(path),
```

That feels pretty hacky, though 😄 I think a better solution might be to reject `--typeshed` CLI arguments if the argument points to a subdirectory of any of the search paths that take higher precedence AND no segment of the custom-typeshed path contains a character that would be illegal in a module name. @sharkdp / @MichaReiser -- does that sound reasonable to you?

---

_Comment by @MichaReiser on 2025-05-27 12:55_

Warning about overlapping search paths seems reasonable in general

---

_Renamed from "[panic] ty panic with `--typeshed`" to "Panic if `--typeshed` is set to a subdirectory of a first-party search path" by @AlexWaygood on 2025-05-27 13:29_

---

_Label `configuration` added by @AlexWaygood on 2025-05-27 13:29_

---

_Comment by @AlexWaygood on 2025-05-30 22:07_

@ntBre and I hacked on this a bit this afternoon in our pairing session. We realized that rejecting any configuration where two search paths overlap would be too naive here:
- Emulating Python's semantics, ty always adds a search path for the current working directory.
- Most users put their first-party code in an `src/` directory, which we'll also add as a search path
- The CWD search path and the `src` overlap: a `package` module located at `src/package.py` could theoretically have the module name `package` or `src.package`, depending on which search path you resolve it relative to.

---

_Comment by @AlexWaygood on 2025-05-31 18:15_

Here's the analogous issue mypy had a while back: https://github.com/python/mypy/issues/4881. And here's what they did to fix it: https://github.com/python/mypy/pull/8644

---

_Comment by @AlexWaygood on 2025-06-06 16:03_

@carljm pointed out in my 1:1 with him just now that it's also common for editable installs to overlap with first-party search paths

---
