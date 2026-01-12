```yaml
number: 19417
title: "[ty] Added support for \"go to definition\" for attribute accesses and keyword arguments"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: goto_attribute
created_at: 2025-07-18T02:36:07Z
updated_at: 2025-07-18T18:34:00Z
url: https://github.com/astral-sh/ruff/pull/19417
synced_at: 2026-01-12T15:56:39Z
```

# [ty] Added support for "go to definition" for attribute accesses and keyword arguments

---

_@UnboundVariable_

This PR builds upon #19371. It addresses a few additional code review suggestions and adds support for attribute accesses (expressions of the form `x.y`) and keyword arguments within call expressions.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-18 02:36_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-18 02:36_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-18 02:36_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-18 02:36_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-18 02:36_

---

_Comment by @github-actions[bot] on 2025-07-18 02:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-07-18 05:49_

---

_Label `ty` added by @MichaReiser on 2025-07-18 05:49_

---

_@sharkdp reviewed on 2025-07-18 07:29_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:531 on 2025-07-18 07:29_

Should intersections also be expanded here by adding all positive elements to `tys`? This way, we could still jump to the definition of `method` in code like this:
```py
class C:
    def method(): ...


def _(x: C | int):
    if isinstance(x, int):
        pass
    else:
        # x is now of type `C & ~int`
        x.method()  # <- click on method here
```

---

_@sharkdp reviewed on 2025-07-18 07:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:616 on 2025-07-18 07:47_

I think we could reuse `Type::to_meta_type` here? Haven't checked if this is identical in all cases, but all tests pass:
```suggestion
        // First, transform the type to its meta type, unless it's already a class-like type.
        let meta_type = match ty {
            Type::ClassLiteral(_) | Type::SubclassOf(_) | Type::GenericAlias(_) => ty,
            _ => ty.to_meta_type(db),
        };
        let class_literal = match meta_type {
            Type::ClassLiteral(class_literal) => class_literal,
            Type::SubclassOf(subclass) => match subclass.subclass_of().into_class() {
                Some(cls) => cls.class_literal(db).0,
                None => continue,
            },
            _ => continue,
        };
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:157 on 2025-07-18 09:06_

Nit: I find this comment more confusing than helpful, especially because `definitions_for_name` isn't a shared helper? It's specific for the name branch (I think)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:206 on 2025-07-18 09:11_

Hmm, this seems a bit unfortunate that we need to go and search the covering node again. Would it make sense to instead store the `CallExpr` in `KeywordArgument` alongside the keyword? 

Getting the function there should be cheaper because `CoveringNode` allows upward traversal whereas calling `covering_node` requires an AST traversal

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:512 on 2025-07-18 09:25_

Can you extend the comment explaining why. I first assumed the next sentence would but that only suggests merging this into the semantic index in the future.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:652 on 2025-07-18 09:28_

Nit: Rust has labeled scopes: This should allow you to remove the `!found_attr` checks
```suggestion
        'scopes: for ancestor in class_literal
            .iter_mro(db, None)
            .filter_map(ClassBase::into_class)
            .map(|cls| cls.class_literal(db).0)
        {
            let class_scope = ancestor.body_scope(db);
            let class_place_table = crate::semantic_index::place_table(db, class_scope);

            // Look for class-level declarations and bindings
            if let Some(place_id) = class_place_table.place_id_by_name(name_str) {
                let use_def = use_def_map(db, class_scope);

                // Check declarations first
                for decl in use_def.all_reachable_declarations(place_id) {
                    if let Some(def) = decl.declaration.definition() {
                        resolved.extend(resolve_definition(db, def, Some(name_str)));
                        found_attr = true;
                        break 'scopes;
                    }
                }

                // If no declarations found, check bindings
                for binding in use_def.all_reachable_bindings(place_id) {
                    if let Some(def) = binding.binding.definition() {
                        resolved.extend(resolve_definition(db, def, Some(name_str)));
                        found_attr = true;
                        break 'scopes;
                    }
                }
                
            }
```

... and more places where  `if !found_attr`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:692 on 2025-07-18 09:31_

Nit: This is not as relevant here because your function isn't a salsa query. But calling `semantic_index` means that this function is invalidated on every change in `file`. Using `place_table` and `use_def_map` can help isolate invalidations because it only requires the calling function (if it is cached) to re-run if the place table or use def map of a specific scope changed. It remains untouched by changes in other scopes.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:843 on 2025-07-18 09:37_

Nit: `chain` and `find`
```suggestion
    // Check regular positional and keyword only parameters
    parameters
        .args
        .iter()
        .chain(&parameters.kwonlyargs)
        .find(|param| param.parameter.name.as_str() == parameter_name)
        .map(|parameter| parameter.name().range())
```



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:872 on 2025-07-18 09:37_

```suggestion
        FileWithRange(FileRange),
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:871 on 2025-07-18 09:37_

```suggestion
        /// The import resolved to a file with an optional specific range
```

Should the range be `Option<TextRange>` or is the comment out dated?

---

_@MichaReiser approved on 2025-07-18 09:38_

---

_@UnboundVariable reviewed on 2025-07-18 17:41_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/goto.rs`:206 on 2025-07-18 17:41_

I'd advise against doing preemptive micro-optimizations here. This is not a hot code path, so using more memory in an attempt to speed it up is probably not the right tradeoff.

---

_@UnboundVariable reviewed on 2025-07-18 17:57_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:616 on 2025-07-18 17:57_

Nice! I wasn't aware the `to_meta_type` existed. Looks like exactly what we need here, and it significantly simplifies the code.

---

_@UnboundVariable reviewed on 2025-07-18 18:04_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:652 on 2025-07-18 18:04_

Nice! I didn't know about that feature in Rust.

---

_@MichaReiser reviewed on 2025-07-18 18:14_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:206 on 2025-07-18 18:14_

I don't think this is a micro optimization. It just seems unnecessary if we can get this information when extracting the GoToDefinitionTarget. It's right there and I find it even less confusing

---

_Merged by @UnboundVariable on 2025-07-18 18:33_

---

_Closed by @UnboundVariable on 2025-07-18 18:33_

---

_Branch deleted on 2025-07-18 18:34_

---
