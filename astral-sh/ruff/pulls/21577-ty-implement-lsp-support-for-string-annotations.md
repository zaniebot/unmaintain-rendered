```yaml
number: 21577
title: "[ty] Implement lsp support for string annotations"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/strattr
created_at: 2025-11-22T13:12:23Z
updated_at: 2025-11-25T13:31:06Z
url: https://github.com/astral-sh/ruff/pull/21577
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Implement lsp support for string annotations

---

_@Gankra_

Fixes https://github.com/astral-sh/ty/issues/1009

## Summary

This adds support for:

* semantic-tokens (syntax highlighting)
* goto-type **(partially implemented, but want to land as-is)**
* goto-declaration
* goto-definition (falls out of goto-declaration)
* hover **(limited by goto-type)**
* find-references
* rename-references (falls out of find-references)

There are 3 major things being introduced here:

* `TypeInferenceBuilder::string_annotations` is a `FxHashSet` of exprs which were determined to be string annotations during inference. It's bubbled up in `extras` to hopefully minimize the overhead as in most contexts it's empty.
  * Very happy to hear if this is too hacky and if I should do something better, but it's IMO important that we get an authoritative answer on whether something is a string annotation or not. 
* `SemanticModel::enter_string_annotation` checks if the expr was marked by `TypeInferenceBuilder::string_annotations` and then parses the subast and produces a sub-SemanticModel that sets `SemanticModel::in_string_annotation_expr`. This expr will be used by the model whenever we need to query e.g. the scope of the current expression (otherwise the code will constantly panic as the subast nodes are not in the current File's AST)
  * This hazard consequently encouraged me to refactor a bunch of code to replace uses of file/db with SemanticModel to minimize hazards (it is no longer as safe to randomly materialize a SemanticModel in the middle of analysis, you need to thread through the one you have in case it has `in_string_annotation_expr` set).
* `GotoTarget::StringAnnotationSubexpr` (and a semantic-tokens impl) which involves invoking `SemanticModel::enter_string_annotation` before invoking the same kind of subroutine a normal expression would.
  * goto-type (and consequently displaying the type in hover) is the main hole here, because we can only get the type iff the string annotation is the entire subexpression (i.e. we can get the type of `"int"` but not the parts of `"int | str"`). This is shippable IMO.

## Test Plan

Messed around in IDE, wrote a ton of tests.

---

_Review requested from @carljm by @Gankra on 2025-11-22 13:12_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-22 13:12_

---

_Review requested from @sharkdp by @Gankra on 2025-11-22 13:12_

---

_Review requested from @dcreager by @Gankra on 2025-11-22 13:12_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-22 13:12_

---

_Label `server` added by @Gankra on 2025-11-22 13:12_

---

_Label `ty` added by @Gankra on 2025-11-22 13:12_

---

_Comment by @Gankra on 2025-11-22 13:13_

...why did a bunch of random comments get deleted. Oh well, it's still just a Draft 🙃 

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 13:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @Gankra on 2025-11-22 13:14_

omg I think when I asked rust-analyzer to add missing members to unwildcard a match it deleted comments on all the existing branches 😭 

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 13:16_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra reviewed on 2025-11-22 13:17_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_model.rs`:313 on 2025-11-22 13:17_

Pre-empting Micha: yes this does fail for various things but the type checker also does this so it's fine

---

_Renamed from "[ty] implement lsp support for highlighting/hovering/goto-ing string annotations" to "[ty] implement lsp support for highlighting/renaming/hovering/goto-ing string annotations" by @Gankra on 2025-11-22 18:30_

---

_Renamed from "[ty] implement lsp support for highlighting/renaming/hovering/goto-ing string annotations" to "[ty] implement lsp support for string annotations" by @Gankra on 2025-11-22 18:35_

---

_Renamed from "[ty] implement lsp support for string annotations" to "[ty] Implement lsp support for string annotations" by @Gankra on 2025-11-22 18:37_

---

_@Gankra reviewed on 2025-11-22 18:39_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:359 on 2025-11-22 18:39_

This is like, the one caveat of this design. Fixing this would require awful hacks or more non-trivial changes to inference.

---

_@Gankra reviewed on 2025-11-22 18:39_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:359 on 2025-11-22 18:39_

(awful hacks would be like... looking up types by-name)

---

_Comment by @Gankra on 2025-11-22 18:43_

I want to make it clear that this is a "high risk" change in that any bugs in this could easily lead to LSP panics. However we could potentially have this functionality toggled behind a feature flag (literally just make `enter_string_annotation` always return None). IMO it should be safe to get into this week's release, if there's issues they should be found pretty fast by people kicking the tires, and then we can trivially disable it if so.

---

_Review request for @carljm removed by @carljm on 2025-11-24 21:19_

---

_Comment by @carljm on 2025-11-24 21:20_

The new string-annotations tracking in inference looks good to me! Didn't review the rest.

---

_Comment by @Gankra on 2025-11-24 21:30_

I'm reasonably confident with the rest of the code. If we wanna get this into the release tomorrow this would be fine to merge without further review (but I'll wait for review for now).

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_model.rs`:259 on 2025-11-25 12:27_

I'm not sure I fully understand those methods and when I'm supposed to use them. 

Like in what sense are they safe where the in-stringified-annotation nodes aren't? When should I use this method over using the ast node directly?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_model.rs`:313 on 2025-11-25 12:31_

lol

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_model.rs`:414 on 2025-11-25 12:34_

So just for my understanding. We now store the expression types of each expression in a stringified literal within the string-literal's scope? 

This is cool!

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:359 on 2025-11-25 12:39_

Okay, maybe it doesn't do yet what I thought it would.

Could we change `ExpressionNodeKey` to be an enum:

```rust
enum ExpressionNodeKey {
    Primary(NodeKey),
    StringifiedAnnotation(Box<{ parent: NodeKey, inner: NodeKey }>)
}
```

(It might even ahve to be a `ThinVec` if stringified annotationos can be nested multiple levels. 

We could then start storing the inferred expressions in type inference and look them up here


Happy to leave this as a follow up

---

_@MichaReiser approved on 2025-11-25 12:41_

This is a huge lift and is great. I think it might even be possible (without too much work), to start storing types for the individual expression parts, see my inline comment. But let's do this in a separate PR

---

_@Gankra reviewed on 2025-11-25 13:22_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:359 on 2025-11-25 13:22_

They cannot be nested, thankfully.

I agree this would be great, are NodeKeys this stable?

(Definitely followup material since it could have more non-trivial perf impact)

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_model.rs`:259 on 2025-11-25 13:24_

The rough heuristic in my mind is "if the node might be handed to the inference subsystem (i.e. trying to query it's scope or type)" (which SemanticModel gatekeeps). If you ever hand these sub-AST nodes to the inference subsystem it will immediately panic because the node isn't part of the file's AST.

---

_@Gankra reviewed on 2025-11-25 13:24_

---

_@MichaReiser reviewed on 2025-11-25 13:26_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:359 on 2025-11-25 13:26_

`NodeKey`'s are stable for as long as the source text doesn't change. We simply iterate over the AST and assign an index to each node (see `parsed_module`)

---

_Merged by @Gankra on 2025-11-25 13:31_

---

_Closed by @Gankra on 2025-11-25 13:31_

---

_Branch deleted on 2025-11-25 13:31_

---
