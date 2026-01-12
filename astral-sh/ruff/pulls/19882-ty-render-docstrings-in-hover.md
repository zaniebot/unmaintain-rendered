```yaml
number: 19882
title: "[ty] render docstrings in hover"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/dochover
created_at: 2025-08-12T18:18:04Z
updated_at: 2025-08-13T15:00:58Z
url: https://github.com/astral-sh/ruff/pull/19882
synced_at: 2026-01-12T15:56:49Z
```

# [ty] render docstrings in hover

---

_@Gankra_

This PR has several components:

* Introduce a Docstring String wrapper type that has render_plaintext and render_markdown methods, to force docstring handlers to pick a rendering format
  * Implement [PEP-257](https://peps.python.org/pep-0257/) docstring trimming for it
  * The markdown rendering just renders the content in a plaintext codeblock for now (followup work)
* Introduce a `DefinitionsOrTargets` type representing the partial evaluation of `GotoTarget::get_definition_targets` to ideally stop at getting `ResolvedDefinitions`
  * Add `declaration_targets`, `definition_targets`, and `docstring` methods to `DefinitionsOrTargets` for the 3 usecases we have for this operation
  * `docstring` is of course the key addition here, it uses the same basic logic that `signature_help` was using: first check the goto-declaration for docstrings, then check the goto-definition for docstrings. 
* Refactor `signature_help` to use the new APIs instead of implementing it itself
  * Not fixed in this PR: an issue I found where `signature_help` will erroneously cache docs between functions that have the same type (hover docs don't have this bug)
* A handful of new tests and additions to tests to add docstrings in various places and see which get caught


Examples of it working with stdlib, third party, and local definitions:
<img width="597" height="120" alt="Screenshot 2025-08-12 at 2 13 55 PM" src="https://github.com/user-attachments/assets/eae54efd-882e-4b50-b5b4-721595224232" />
<img width="598" height="281" alt="Screenshot 2025-08-12 at 2 14 06 PM" src="https://github.com/user-attachments/assets/5c9740d5-a06b-4c22-9349-da6eb9a9ba5a" />
<img width="327" height="180" alt="Screenshot 2025-08-12 at 2 14 18 PM" src="https://github.com/user-attachments/assets/3b5647b9-2cdd-4c5b-bb7d-da23bff1bcb5" />

Notably modules don't work yet (followup work):
<img width="224" height="83" alt="Screenshot 2025-08-12 at 2 14 37 PM" src="https://github.com/user-attachments/assets/7e9dcb70-a10e-46d9-a85c-9fe52c3b7e7b" />

Notably we don't show docs for an item if you hover its actual definition (followup work, but also, not the most important):
<img width="324" height="69" alt="Screenshot 2025-08-12 at 2 16 54 PM" src="https://github.com/user-attachments/assets/d4ddcdd8-c3fc-4120-ac93-cefdf57933b4" />


---

_Review requested from @carljm by @Gankra on 2025-08-12 18:18_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-12 18:18_

---

_Review requested from @sharkdp by @Gankra on 2025-08-12 18:18_

---

_Review requested from @dcreager by @Gankra on 2025-08-12 18:18_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-12 18:18_

---

_Label `server` added by @Gankra on 2025-08-12 18:18_

---

_Label `ty` added by @Gankra on 2025-08-12 18:18_

---

_Comment by @github-actions[bot] on 2025-08-12 18:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 14:57:30.986477337 +0000
+++ new-output.txt	2025-08-13 14:57:31.053477542 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(b5023)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(244b3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-12 18:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@Gankra reviewed on 2025-08-12 18:28_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:332 on 2025-08-12 18:28_

This result is a disappointment but a pre-existing issue that we don't really distinguish a class from an invocation of the initializer (future work).

---

_@Gankra reviewed on 2025-08-12 18:29_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:541 on 2025-08-12 18:29_

This doesn't change the result at all but I changed this test in case one day our hover gets smart enough to see and render these docs.

---

_@Gankra reviewed on 2025-08-12 18:30_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:580 on 2025-08-12 18:30_

Similarly this doesn't change anything, but I added docs just in case something got smarter

---

_@Gankra reviewed on 2025-08-12 18:31_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:624 on 2025-08-12 18:31_

Future work, this should render eventually.

---

_@Gankra reviewed on 2025-08-12 18:33_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:666 on 2025-08-12 18:33_

This one's even worse and produces no result because apparently hovering an import doesn't yield the module (even though I fixed goto-def on it in a previous PR). I assume this is because hover currently insists it needs the type of the hovered item, not the def/decl.

---

_@Gankra reviewed on 2025-08-12 18:34_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:977 on 2025-08-12 18:34_

Another "maybe one day" change

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:81 on 2025-08-13 07:33_

I know this is pre-existing code, so I'm fine deferring it but I wonder if it makes sense to short-circuit if we parsed parameters by at least one style because most code will only use one convention.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:133 on 2025-08-13 07:38_

```suggestion
    // (all python whitespace is ascii so we can just remove that many bytes from the front).
```

I found this comment slightly confusing because whitespace in strings can be unicode whitespace. It's only whitespace in indentations (and between expressions etc) that are all ASCII

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:96 on 2025-08-13 07:40_

Yay, our second implementation of this algorithm (it makes sense for them to be different):

https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_python_formatter/src/string/docstring.rs#L27-L114

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:187 on 2025-08-13 07:43_

Nit: The stub mapper type still feels a bit overkill to me. It feels like it could easily be replaced by an enum with two variants (map, don't map) and a method (that takes `db` as an argument) that does the mapping. 

Maybe something for a follow up PR?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:154 on 2025-08-13 07:45_

Could you add some documentation to these two variants. Without digging through `ResolvedDefinition` and `NavigationTargets`, it was hard to understand what each represents (and I did such a bad job to not document `NavigationTargets`)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/hover.rs`:104 on 2025-08-13 07:53_

My idea behind making `HoverContent` an enum was that a `Hover` can be made up out of different building blocks where each building block knows how to render itself. A docstring could be such a building block and so is `Type`. Following that idea, I'd probably make `Docstring` its own enum variant. However, I'm not sure if what I had in mind back then does scale well and it heavily depends on how tightly the rendering is coupled between and maybe it's best if collapse `HoverContent` into `Hover`

---

_Review comment by @MichaReiser on `crates/ty_ide/src/hover.rs`:136 on 2025-08-13 07:54_

Nit: Would it make sense for `docstring` to have a `render(MarkupKind)` method instead. It seems that would fit nicer together with `MarkupKind` and also allows to use the `fenced_code_block` helper

---

_@MichaReiser approved on 2025-08-13 07:57_

This is excellent. Thank you for working on this. I only have a few detail comments that are at your own discretion. 

---

_@Gankra reviewed on 2025-08-13 14:35_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:187 on 2025-08-13 14:35_

I agree, I have several followup refactors planned here.

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:81 on 2025-08-13 14:39_

We actually have a test checked in for the mixed behaviour, so i guess @UnboundVariable thought it was something that we'll see in the wild?

---

_@Gankra reviewed on 2025-08-13 14:39_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:96 on 2025-08-13 14:40_

In retrospect I really should have checked because of course we do 😓 

---

_@Gankra reviewed on 2025-08-13 14:40_

---

_Merged by @Gankra on 2025-08-13 14:59_

---

_Closed by @Gankra on 2025-08-13 14:59_

---

_Branch deleted on 2025-08-13 14:59_

---
