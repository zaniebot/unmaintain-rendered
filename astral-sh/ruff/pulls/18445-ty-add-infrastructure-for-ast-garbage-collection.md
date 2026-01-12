```yaml
number: 18445
title: "[ty] Add infrastructure for AST garbage collection"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/ast-node-gc-infra
created_at: 2025-06-03T17:17:15Z
updated_at: 2025-06-11T10:45:08Z
url: https://github.com/astral-sh/ruff/pull/18445
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Add infrastructure for AST garbage collection

---

_@ibraheemdev_

## Summary

https://github.com/astral-sh/ty/issues/214 will require a couple invasive changes that I would like to get merged even before garbage collection is fully implemented (to avoid rebasing):
- `ParsedModule` can no longer be dereferenced directly. Instead you need to load a `ParsedModuleRef` to access the AST, which requires a reference to the salsa database (as it may require re-parsing the AST if it was collected).
- `AstNodeRef` can only be dereferenced with the `node` method, which takes a reference to the `ParsedModuleRef`. This allows us to encode the fact that ASTs do not live as long as the database and may be collected as soon a given instance of a `ParsedModuleRef` is dropped. There are a number of places where we currently merge the `'db` and `'ast` lifetimes, so this requires giving some types/functions two separate lifetime parameters.


---

_Review requested from @carljm by @ibraheemdev on 2025-06-03 17:17_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-06-03 17:17_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-06-03 17:17_

---

_Review requested from @dcreager by @ibraheemdev on 2025-06-03 17:17_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-06-03 17:17_

---

_Comment by @github-actions[bot] on 2025-06-03 17:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-03 17:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood reviewed on 2025-06-03 17:37_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:15 on 2025-06-03 17:37_

should fix the compile error on the MSRV build:

```suggestion
    let parsed = parsed_module(db.upcast(), file).load(db.upcast());
```

---

_Label `ty` added by @AlexWaygood on 2025-06-03 18:12_

---

_Renamed from "Add infrastructure for AST garbage collection" to "[ty] Add infrastructure for AST garbage collection" by @AlexWaygood on 2025-06-03 18:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/bind.rs`:2295 on 2025-06-04 07:45_

One of the added benefits of having pass `parsed_module` now is that it becomes more appearant when a function does a cross-file AST access (which always need to be encapsualted by a query to avoid cross-file query dependencies)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:759 on 2025-06-04 07:46_

It's probably a bit excessive to document this everywhere but we should document it at least for `AstNodeRef` that the method panics if `module` belongs to a different file.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:764 on 2025-06-04 07:47_

Should this method take a `ParsedModuleRef`. Or what's the reason that this method resolves it internally but `node` doesn't?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:997 on 2025-06-04 07:47_

Same here: Would it make sense to change this method to take a `ParsedModuleRef` argument? 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:2045 on 2025-06-04 07:48_

I think it would be more consistent if this method take a `ParsedModuleRef` argument, similar to some other range functions.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/context.rs`:164 on 2025-06-04 07:49_

I think it could make sense for `InferContext` to store the parsed module as a field, given that it is only created inside type inference builder where we always have a parsed module.

We could then also expose it as a method so that we can cheaply retrieve the parsed module everywhere where we have an `InferContext`.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:647 on 2025-06-04 07:55_

Should we just clone the ref to avoid the additional lifetime argument? This should be cheap, given that it is a thin wrapper around an `Arc`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:4864 on 2025-06-04 07:57_


```suggestion
        self.infer_target(target, iter, |builder, iter_expr| {
            // TODO: `infer_comprehension_definition` reports a diagnostic if `iter_ty` isn't iterable
            //  but only if the target is a name. We should report a diagnostic here if the target isn't a name:
            //  `[... for a.x in not_iterable]
            if is_first {
                infer_same_file_expression_type(
                    builder.db(),
                    builder.index.expression(iter_expr),
                    builder.module,
                )
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:77 on 2025-06-04 07:58_

Should we just clone the `ParsedModuleRef` here to avoid the extra lifetime?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:66 on 2025-06-04 07:59_

Let's document that this method panics if `parsed` isn't a reference to the same `ParsedModule` (including revision) as for which the `AstNodeRef` was created with.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:5070 on 2025-06-04 08:05_

@carljm it seems problematic to me that this `Type` method access the AST. Are there some guardrails in place that I can't see from the method's signature that prevent this from creating cross-module dependencies? Maybe there is because we pass in scope? Should we make this method `pub(crate)`

---

_@MichaReiser approved on 2025-06-04 08:07_

Nice. This looks good to me. 

The main thing that's unclear to me is when/how you decided which functions resolve `ParsedModuleRef` internally vs taking it as an argument. Some context there would be helpful. 

Otherwise, I think it would be good to add `ParsedModuleRef` to `InferContext` and use the module from the `InferContext` where possible.

---

_Comment by @ibraheemdev on 2025-06-04 12:06_

> The main thing that's unclear to me is when/how you decided which functions resolve `ParsedModuleRef` internally vs taking it as an argument.

Generally if the function returned a value referencing the AST it needs to take a reference to the parsed module, otherwise it's fine to resolve internally. A lot of the functions already resolved `semantic_index` internally so it seems there wasn't a performance concern (and we could probably create a query that merges both of those if it becomes noticeable). A lot of the methods could take a `ParsedModuleRef` and avoid performing the query, but doing it this way also made the changes easier to make programmatically.

---

_Comment by @ibraheemdev on 2025-06-04 12:53_

I went through and made the code more consistent in terms of only calling `parsed_module` in a query or in a function where we already call `semantic_index`.

---

_@ibraheemdev reviewed on 2025-06-04 12:56_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/semantic_index/builder.rs`:77 on 2025-06-04 12:56_

The separate lifetime makes it easier to work with internally because it can be copied around without borrowing `self`, which can interfere with calling methods that take` &mut self` as well as a reference to the module. Otherwise we end up having to clone the module again before calling mutable methods.

---

_Merged by @ibraheemdev on 2025-06-05 15:43_

---

_Closed by @ibraheemdev on 2025-06-05 15:43_

---

_Branch deleted on 2025-06-05 15:43_

---

_@carljm reviewed on 2025-06-05 23:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5070 on 2025-06-05 23:57_

I think this case is currently fine in practice, because we pass in `scope_id`, and this is always a `scope_id` from the file we are currently inferring, never cross-file. You can see this from the fact that every call site of `Type::in_type_expression` in `TypeInferenceBuilder` passes in `self.scope()` for the scope argument.

It would definitely be fine to make this method `pub(crate)`, though I don't know how much that would really help prevent a possible future mis-use. We could also add a note to the method doc comment.

Not sure what else we could do here...

---

_@carljm reviewed on 2025-06-06 00:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5070 on 2025-06-06 00:06_

https://github.com/astral-sh/ruff/pull/18488 -- open to additional suggestions

---

_Label `internal` added by @dhruvmanila on 2025-06-11 10:45_

---
