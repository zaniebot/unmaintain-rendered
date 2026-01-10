```yaml
number: 11963
title: "Introduce `HasTy` trait and `SemanticModel` facade"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: semantic-model-api
created_at: 2024-06-21T10:43:28Z
updated_at: 2024-07-01T12:48:29Z
url: https://github.com/astral-sh/ruff/pull/11963
synced_at: 2026-01-10T21:56:00Z
```

# Introduce `HasTy` trait and `SemanticModel` facade

---

_Pull request opened by @MichaReiser on 2024-06-21 10:43_

## Summary

This PR introduces a new `HasTy` trait that AST nodes implement that support looking up their type. 

It also introduces a `SemanticModel` facade that acts as the public interface of the semantic analysis. 

I think the API could be better. It's a bit awkward that you need to construct a `typing_context` and pass that to type operations. I think we need to spend some more time on fully designing the API that we want to provide to lint rules.

## Other changes

I changed the symbol table builder to not introduce new symbols for reads, if a parent scope defines a symbol with that name. There's probably a better and more correct way of implementing this but I needed a fix or using `typing.override` as a decorator would always resolve to `Unbound`. 

## Follow-up work

* Implement `HasTy` for `Alias` 
* Merge `HasTy` with a `SemanticNode` trait that also gives access to the node's symbol (only `Some` for `ExprName` and `Alias`), or a node's scope. I decided not to implement this as part of this PR because it wasn't exactly clear to me what `scope` would return. Is it the enclosing scope or the scope that the node defines? 

## Test plan

I updated the `lint_bad_override` rule to use the new API. It's not optimized for performance. For example, we look up the `typing.override` type for every class definition. 

```rust
fn lint_bad_override(context: &SemanticLintContext, class: &ast::StmtClassDef) {
    let semantic = &context.semantic;
    let typing_context = semantic.typing_context();

    // TODO we should have a special marker on the real typing module (from typeshed) so if you
    //   have your own "typing" module in your project, we don't consider it THE typing module (and
    //   same for other stdlib modules that our lint rules care about)
    let Some(typing) = semantic.resolve_module(ModuleName::new("typing").unwrap()) else {
        return;
    };

    let Some(typing_override) = semantic.public_symbol(typing, "override") else {
        return;
    };

    let override_ty = semantic.public_symbol_ty(typing_override);

    let class_ty = class.ty(semantic);

    for function in class
        .body
        .iter()
        .filter_map(|stmt| stmt.as_function_def_stmt())
    {
        let Type::Function(ty) = function.ty(semantic) else {
            return;
        };

        if ty.has_decorator(&typing_context, override_ty) {
            let method_name = ty.name(&typing_context);
            if class_ty
                .inherited_class_member(&typing_context, method_name)
                .is_none()
            {
                // TODO should have a qualname() method to support nested classes
                context.push_diagnostic(
                    format!(
                        "Method {}.{} is decorated with `typing.override` but does not override any base class method",
                        class_ty.name(&typing_context),
                        method_name,
                    ));
            }
        }
    }
}
```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module/resolver.rs`:47 on 2024-06-21 10:56_

* Tracing is hard. It needs to be `entered` or the span exits immediately. 
* I had to use `_span` because rust drops `_` immediately?

---

_Comment by @github-actions[bot] on 2024-06-21 11:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---

_Label `red-knot` added by @MichaReiser on 2024-06-21 11:19_

---

_@MichaReiser reviewed on 2024-06-21 11:19_

---

_Marked ready for review by @MichaReiser on 2024-06-21 11:22_

---

_Review requested from @carljm by @MichaReiser on 2024-06-21 11:39_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:44 on 2024-06-21 17:05_

Not necessary to do in this PR, but if we will need this boilerplate in all functions we want to trace, it may be worth our own macro to a) consistently get it right and b) make it less noisy for reading the code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/lib.rs`:6 on 2024-06-21 17:07_

Random sidenote, but I'm a bit confused about import-sorting in rustfmt. I feel like sometimes rustfmt does move imports around, but then I also see changes like this one that look like manual changes?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:84 on 2024-06-21 17:11_

👍 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:96 on 2024-06-21 17:16_

I think this might render `Scope.definition` field unnecessary? (And maybe also `Scope.defining_symbol`?)

And to be clear, I think that's good -- I think it's cleaner to just handle this directly in terms of nodes and leave Definition out of it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:115 on 2024-06-21 17:24_

It seemed odd to me that the compiler would need an explicit lifetime here, when the lifetime is not otherwise involved in the function, but I guess this is a limitation specifically related to `impl Trait` arguments that has been lifted in unstable, but not yet in stable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:32 on 2024-06-21 17:50_

Maybe name these the same as the corresponding fields in `SemanticIndex`? This makes it clearer to the reader that they are the same thing. And if the name is better/clearer in `SemanticIndex`, it's also better here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-21 17:59_

Ah! I had the nagging feeling reviewing the last PR that something else wasn't quite right with the "walk up scopes looking for one that has the symbol" logic added there, and now seeing this I realize what it is.

This approach won't work when we add control flow and implement support for the `nonlocal` statement, because then you can have symbols in a scope that may be defined in the scope, but references to the name that don't see the definition (because they are before it, or because it's conditional) are still nonlocal references to an outer scope. So the approach for nonlocal names has to respect control flow, it can't just walk up scopes directly; we need to insert a Definition at the start of the scope (in place of Unbound) which indicates "refer to outer scope public type."

It makes sense to have a temporary solution in this PR to make the lint rule work, even without control flow. But I would much prefer if the temporary solution didn't touch the symbol table in this way that will have to be reverted later. Instead I think you could change the "walk up scopes" name resolution logic you added in type inference in the last PR, so that instead of "walk up scopes if symbol doesn't exist" it's "walk up scopes if symbol does not have definitions in this scope." This will give the same temporary behavior, but it will localize the changes into the code that will need to change anyway when adding control flow.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_model.rs`:23 on 2024-06-21 18:32_

Not sure if this is a good idea, feel free to ignore, but it occurred to me that we do have a `Type::Module`and we could reduce the number of types exposed by the semantic model if this also returned a `Type`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_model.rs`:152 on 2024-06-21 18:40_

It feels weird that we have to go the inner scope of the function, then walk up one or two scopes, worrying about details like AnnotationScope, just to find the scope in which the FunctionDef node occurs. We already keep a map from node to scope it occurs in for expressions, and there are a lot more of those; maybe we should keep the same mapping for some statements, too?

---

_@carljm reviewed on 2024-06-21 18:45_

---

_Comment by @carljm on 2024-06-21 18:57_

This looks great!

> `SemanticNode` trait that also gives access to the node's symbol (only `Some` for `ExprName` and `Alias`)

I think this makes sense. The only odd thing about it is what a small percentage of nodes it is `Some` for, but this isn't an actual problem. I do wonder if there's a strong reason to do this now, or if we should wait until we are implementing a use case that needs it, to better inform the design. It seems possible that any use case that would need this is actually something the semantic model should provide directly as a higher-level abstraction, but I'm not sure.

> or a node's scope. I decided not to implement this as part of this PR because it wasn't exactly clear to me what scope would return. Is it the enclosing scope or the scope that the node defines?

The two are quite different, and both possibly useful in different circumstances, which probably suggests the name should be clear, and neither one should be named simply `scope`. But I have the same question about this: maybe better to design it alongside a clear motivating use case?

---

_Comment by @carljm on 2024-06-21 19:06_

On second thought I don't see any issue with adding nice APIs now to get from node to symbol/scope; it'll be useful internally regardless of whether we encourage lint rules to directly use those APIs. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-21 19:09_

I considered filtering in the type inference but having types in the symbol table that come from the parent scope feels like a huge foot gun because we would need to filter them whenever we lookup a symbol by name. 

---

_@MichaReiser reviewed on 2024-06-21 19:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_model.rs`:23 on 2024-06-21 19:11_

Hmm not sure. But I can probably remove the method for now and revisit when I look into extracting dependencies.

---

_@MichaReiser reviewed on 2024-06-21 19:13_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_model.rs`:152 on 2024-06-21 19:13_

I agree that the code here is painful but I'm worried about tracking too much data over just recomputing when necessary. 

An alternative I considered that could also make things more consistent is that the `Definition -> Scope` map doesn't track the scope the node introduces, but the scope in which they are defined. This way, the scope returned is the same for all nodes, which isn't the case today where for functions, it's the function scope and for imports, it's the enclosing scope.

---

_Comment by @MichaReiser on 2024-06-21 19:15_

> On second thought I don't see any issue with adding nice APIs now to get from node to symbol/scope; it'll be useful internally regardless of whether we encourage lint rules to directly use those APIs.

I'll hold off for now until we have a clear use case. We also need to figure out how we can avoid duplicating code just for internal/external API (the `ty` method takes a `SemanticModel`, which we don't really have internally). 

---

_@carljm reviewed on 2024-06-21 19:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-21 19:16_

I don't understand. We don't have types in the symbol table at all. I'm not clear where the foot gun or filtering worry is. 

I explained above why we must have symbols in the symbol table if used in that scope, whether defined in that scope or not.

If you want to leave this PR this way that's ok, I'll just have to revert this change again in the PR that adds control flow. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:44 on 2024-06-21 19:17_

I don't mind it too much. One thing I think would be worth exploring is if it's possible to add a `tracing` feature to `salsa-macros` or change the macro so that it applies other attributes correctly

---

_@MichaReiser reviewed on 2024-06-21 19:17_

---

_@MichaReiser reviewed on 2024-06-21 19:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:96 on 2024-06-21 19:20_

I think that's different. It maps from `Function -> Scope`. But `Scope.definitions` and `Scope.defining_symbol` map in the other direction. It's the equivalent of the `expression -> Scope` mapping but for `NodeWithScope` (or later, definitions)



---

_@MichaReiser reviewed on 2024-06-21 19:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-21 19:28_

What I'm concerned about is that all call-sites of `symbol_table.symbol_by_name` will have to check whether the returned symbol is actually defined in that scope. I find it rather unexpected to find symbols in a scope that are inherited from the parent and it seems like an easy source for bugs. 

I'm okay patching this up in the call site, but how do we avoid that we don't have to do this in multiple places. For example, how can we avoid that we don't need to do it in the `SemanticNode::symbol` implementation of `ExprName` and in the type inference code? I guess one solution is to just implement it on `ExprName`, but it is then important that we only use that method too perform the lookup making it even more important that we don't expose `symbol_table` directly.

---

_@carljm reviewed on 2024-06-21 20:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-21 20:22_

The symbol in the inner scope is a separate symbol from the symbol in the outer scope. In some cases one of a symbol's definitions may refer to the corresponding symbol in the outer scope, but this will be correctly reflected in the definitions; we'll have a `Definition:Nonlocal` or similar at the start of control flow of the inner scope, whose type is resolved by looking at the public type of the corresponding symbol in the outer scope.

This precisely maps to how free variables are handled in the runtime: the two scopes have separate symbols of the same name, with separate local variable storage in the frame for each scope. At the start of the inner frame the local variable storage for a free variable is populated with a "cell" containing the value from the outer scope.

So I don't think it's correct to assume that callers even _should_ do the filtering you're describing, or that it will be a bug if they don't do it; it depends entirely on why the caller is asking for a symbol. Symbols exist in a scope, and if the caller asks for the symbol in scope X, they should get that symbol, not the one in some other outer scope. If the caller then wants to know where the symbol's value might have been defined, they should use Definitions to discover that, and Definitions will give them the right answer, possibly via `Definition::Nonlocal`. (But in most cases, the caller shouldn't be worrying about this at all, they should just ask for the type of the symbol, and type inference will already have handled the definitions lookup for them.)

Even in type inference name lookup, the filtering we are discussing adding there is only a temporary hack to make the lint rule work until we add control flow. Once we have control flow, even type inference won't do the filtering (or the walking up scopes that you currently do) when inferring the type of an `ExprName`; it will just look for the reachable definition(s) in the local scope, one of which might be `Definition::Nonlocal`, and resolve the types of those definitions appropriately.

If you have a specific use case for `Node::symbol` in mind that you are worried might do the wrong thing for free variables, we can discuss how to avoid that.


---

_@carljm reviewed on 2024-06-21 21:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-21 21:00_

Another way of framing this: Python's name lookup rules are inherently quite complex (e.g. with `nonlocal`, `global`, the special rules about class scopes and annotation scopes, etc). Regardless of how we handle this particular case of free variables, we can't get rid of this complexity.

One way of handling this, which is the direction you're headed in with this change, is to embed all of these name lookup rules inside how we build the symbol table itself. But this still means we have to duplicate this logic both in the symbol table builder and in every user of it. In this PR, the scope-walking is now duplicated both here and in type inference, and it has the same bugs regarding class and annotation scopes in both places, that would now have to be fixed separately in both places. So if we have multiple callers needing to do name lookup, this approach doesn't avoid duplication of the logic, it just adds one additional place (the symbol table builder) where the same logic has to exist.

This approach also has trouble with `nonlocal` or `global` symbols that are defined both in the inner and outer scope; we can't treat these as just a single symbol, without ignoring the definition(s) in the inner scope, or else having outer scopes depend on inner ones, and if we treat them as two separate symbols, then we are right back to needing `Definition::Nonlocal` anyway.

---

_@MichaReiser reviewed on 2024-06-22 16:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lib.rs`:6 on 2024-06-22 16:54_

I think part of it is RustRover's import formating (that organizes imports into group) and only then rustfmt that sorts the imports per group

---

_@MichaReiser reviewed on 2024-06-22 16:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:96 on 2024-06-22 16:56_

You're right. I removed `definition` and `defining_symbol` as they're currently unused. I also removed the extra `IndexVec` and added a `node` field on `scope`. 

---

_@MichaReiser reviewed on 2024-06-22 16:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:115 on 2024-06-22 16:57_

Yeah, it's a bit annoying but not much

---

_@MichaReiser reviewed on 2024-06-22 16:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-22 16:58_

I changed it. I'm still hesitant about creating symbol table entries for symbols that don't exist in that scope (and I think that's different from `nonlocal` because we would create a local symbol in that case). But we can discuss it in detail when you implement `nonlocal`.

---

_@carljm reviewed on 2024-06-22 17:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_model.rs`:152 on 2024-06-22 17:02_

I don't think the enclosing scope alone is enough information to find the correct inner scope if there are eg multiple classes with the same name defined in a scope. I think in the end we may want to track both separately for the cases where each can be relevant.

Given that we already track enclosing-scope for expressions, which are much more numerous than function or class defs, it doesn't seem like it would add much at all in terms of overall storage?

---

_@MichaReiser reviewed on 2024-06-22 17:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-22 17:08_

Another example where I think we need the filtering. `public_symbol` 

https://github.com/astral-sh/ruff/blob/085c33d48914db75e9010d86f03ffdabf2898151/crates/red_knot_python_semantic/src/semantic_index.rs#L61-L70

It currently returns `Some` when looking up `x` in

```python
print(x)
```


---

_@MichaReiser reviewed on 2024-06-22 17:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_model.rs`:152 on 2024-06-22 17:21_

I agree that it wouldn't add much. But performance dies by 100 cuts. It's an extra allocation for file and it requires writing extra data. That's not much, but also not nothing. I would like to avoid tracking redundant that where we can and I think here we can avoid it, by doing some simple traversal. I don't expect this code to get spread out over the entire code base. So handling type params here seems fine.

---

_@carljm reviewed on 2024-06-22 17:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:141 on 2024-06-22 17:31_

There's no need for filtering there. It's fine if the symbol returns Some, and its only definition in that scope is unbound. We have to handle the possibility of unbound regardless, so this doesn't introduce any new problem or case to handle.

In the public symbols case it would be fine either way; in most typical lookups of names used within the scope, it will be simpler if we only have to handle a definition of Unbound or Nonlocal, rather than having to handle both that case and "symbol doesn't exist" (in the same way.)

---

_@carljm approved on 2024-06-24 13:39_

Looks good!

---

_Merged by @MichaReiser on 2024-07-01 12:48_

---

_Closed by @MichaReiser on 2024-07-01 12:48_

---

_Branch deleted on 2024-07-01 12:48_

---
