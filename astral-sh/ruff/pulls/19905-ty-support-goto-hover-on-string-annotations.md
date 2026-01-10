```yaml
number: 19905
title: "[ty] support goto/hover on string annotations"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/goto-str
created_at: 2025-08-13T22:36:43Z
updated_at: 2025-11-21T20:16:32Z
url: https://github.com/astral-sh/ruff/pull/19905
synced_at: 2026-01-10T16:48:01Z
```

# [ty] support goto/hover on string annotations

---

_Pull request opened by @Gankra on 2025-08-13 22:36_

The root issues with this implementation is that ty_python_semantic doesn't really provide a way to ask:
* ~~hey there's a string literal here, is this a type annotation?~~
* hey this substring of a string annotation, what type is it?

This means that we:
* ~~currently assume *every* string literal that is shaped like a type annotation *is* one (and *not* a `str`)~~
* ~~can't implement `goto_type_definition` for types we find in type annotations~~
  * can only report `goto_type_definition` to be the type of the whole string, and not the substring 
* ~~find-and-replace functionality may erroneously edit random strings that happen to match?~~

Thankfully I don't *think* adding the necessary APIs to ty_python_semantic (or whatever) is actually that hard..? perhaps famous last words.

Fixes https://github.com/astral-sh/ty/issues/1009

---

_@Gankra reviewed on 2025-08-13 22:37_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto_type_definition.rs`:229 on 2025-08-13 22:37_

This is the main Problematic thing about this implementation -- we don't ask the type checker if this string actually was a string annotation.

---

_Comment by @github-actions[bot] on 2025-08-13 22:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 00:59:43.392643714 +0000
+++ new-output.txt	2025-08-14 00:59:43.459644200 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1a4cb)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(f2b2)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @Gankra on 2025-08-13 22:39_

<img width="851" height="356" alt="Screenshot 2025-08-13 at 6 39 16 PM" src="https://github.com/user-attachments/assets/3555ad9b-65e1-41ca-884f-6e1bd125313d" />

<img width="711" height="224" alt="Screenshot 2025-08-13 at 6 39 31 PM" src="https://github.com/user-attachments/assets/1d693fcb-f935-4cde-939b-503cf01d5a95" />


---

_Comment by @github-actions[bot] on 2025-08-13 22:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-08-13 22:41_

---

_Label `ty` added by @AlexWaygood on 2025-08-13 22:41_

---

_Comment by @Gankra on 2025-08-14 00:59_

I thought of a way to technically ask the inference engine "hey is this string an annotation?" (ask what the string infers to, and check if that's literally the string it is. if it's not, it must be a type annotation.)

This leaves the only real issue being "way to ask the type of a substring of an annotation". I think this is technically shippable but a bit dubious.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:690 on 2025-08-14 06:55_

This check will return false for strings longer than 4096 bytes

---

_@MichaReiser reviewed on 2025-08-14 07:04_

This doesn't look to horrible and I wouldn't mind merging it, except that it's probably not that useful either because it is limited to names. 


Looking at this, it's also not a go to definition specific problem. E.g. rename also doesn't consider references in stringified annotations (I think). 

I don't have a great idea for how to support this holisticly from the top of my mind. One idea that we floated in the past and might work here is to support virtual documents. 

A virtual document (or file) is a snippet from within another file but it gets its own `File` id (it probably has to be an id layered on top of `File` but conceptually its an id that can be a real file or a virtual file). 

We'll need a similar feature if we ever want to support embedded code, e.g. some SQL inside python. We wouldn't want to re-parse that sql statement for formatting and linting, it should only be parsed once. 

I think this idea is also similar to how rust analyzer handles macros. 

https://github.com/rust-lang/rust-analyzer/blob/59ef84506deb45e6d4336935d0d4be54c3386814/crates/hir-expand/src/lib.rs#L1074-L1077


This is obviously a pretty big change. I'd be okay punting on this for the beta but we should create an issue outlining the problems we've run into. 

---

_Comment by @Gankra on 2025-11-21 20:16_

bitrotted to hell, will restart this from scratch with months of experience under my belt

---

_Closed by @Gankra on 2025-11-21 20:16_

---
