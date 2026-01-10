```yaml
number: 12988
title: Add API to emit type-checking diagnostics
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: type-check
created_at: 2024-08-19T12:23:20Z
updated_at: 2024-08-20T07:37:10Z
url: https://github.com/astral-sh/ruff/pull/12988
synced_at: 2026-01-10T21:38:32Z
```

# Add API to emit type-checking diagnostics

---

_Pull request opened by @MichaReiser on 2024-08-19 12:23_

## Summary

This PR introduces a new API to `TypeInferenceBuilder` to emit type-checking diagnostics. 

The API is intentionally kept simple. The PR doesn't aim to resolve on how we represent rules/violations in Red Knot. 

## Design considerations

This PR wraps diagnostics in an `Arc` to make them cheap-cloneable. It's desired to have cheap-cloneable diagnostics
because we have to copy them between `infer_expression` -> `infer_definition` -> `infer_scope`... 

An alternative design that I think is worth exploring once the Salsa tables refactor lands is to introduce a 
`TypeCheckDiagnosticIngredient` that has a single field, the diagnostic. The advantage of this approach would be that
we get "Arena" allocation for the diagnostics. The downside is that diagnostic must be verified everytime the revision changes which
isn't entirely free. 

## Rule Selector

Long term, I think we want to have a `rule_selector(file) -> RuleSelector` query that resolves the enabled rules per file. 
As said before, this PR doesn't try to resolve how and which rules are enabled per file. That's why I omitted this part for now but the idea is that 
`push_diagnostic` would call the `rule_selector` instead of just calling `is_file_open`. 

## Example diagnostic
I implemented an example diagnostic that flags unresolved imports.

## Why do we need `is_file_open` 

The check for `is_file_open` is mainly an optimization to avoid generating a lot of diagnostics that will never be shown. 


## Test Plan

I added a small unit test. I also verified that running the CLI shows the diagnostic.


---

_Label `red-knot` added by @MichaReiser on 2024-08-19 12:37_

---

_Marked ready for review by @MichaReiser on 2024-08-19 12:38_

---

_Review requested from @carljm by @MichaReiser on 2024-08-19 12:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-19 12:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:48 on 2024-08-19 12:48_

Could we call this method `with_range()`? The name is currently the same as `ruff_text_size::Ranged::range()`, so I assumed it was going to return the range rather than set the range

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:46 on 2024-08-19 12:50_

Some of this comment feels more like a description of the internal implementation rather than a description of how to use the type; I'd move it to a comment above the field rather than a comment above the type:

```suggestion
/// A collection of type check diagnostics.
#[derive(Default, Eq, PartialEq)]
pub struct TypeCheckDiagnostics {
	/// The diagnostics are wrapped in an `Arc` because they need to be cloned multiple times
	/// when going from `infer_expression` to `check_file`. We could consider
	/// making [`TypeCheckDiagnostic`] a Salsa struct to have them Arena-allocated (once the Tables refactor is done).
	/// Using Salsa struct does have the downside that it leaks the Salsa dependency into diagnostics and
	/// each Salsa-struct comes with an overhead.
    inner: Vec<std::sync::Arc<TypeCheckDiagnostic>>,
}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:55 on 2024-08-19 12:59_

Hmm, this method doesn't actually build a diagnostic directly -- it creates a builder that can be used to build a diagnostic. Maybe it should be called `diagnostic_builder()`?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:991 on 2024-08-19 13:08_

I'm quitting Astral if we use CamelCase for our red-knot error codes 😆

I also find the builder interface that you've added in `diagnostics.rs` a little confusing, because:
- `push_diagnostic()` doesn't actually push any diagnostics onto the stack; it returns a builder that can be used to push diagnostics onto the stack
- we're doing two distinct things in the `finish()` method:
  - We're "finishing" the diagnostic that the builder is building
  - We're pushing the diagnostic onto the stack of finished diagnostics

  I'd much prefer it if we could just do those in two steps so it's a bit more explicit what's going on

My preferred API would be something like this:

```rs
            if let Some(builder) = self.diagnostic_builder(node, "reportMissingImport") {
                let diagnostic = builder.finish(format!("Import '{module_name}' could not be resolved."));
                self.push_diagnostic(diagnostic);
            }
```

---

_@AlexWaygood reviewed on 2024-08-19 13:10_

Thanks! I'm not 100% sure about some of these interfaces. Some thoughts below:

---

_@AlexWaygood reviewed on 2024-08-19 13:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace.rs`:491 on 2024-08-19 13:13_

```suggestion
        assert_eq!(check_file(&db, file), Vec::<String>::new());
```

---

_@MichaReiser reviewed on 2024-08-19 13:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:48 on 2024-08-19 13:15_

This is intentional. It's common in builders to call the methods the same as the field names. 

---

_Comment by @github-actions[bot] on 2024-08-19 13:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood reviewed on 2024-08-19 13:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:48 on 2024-08-19 13:21_

Fair enough -- I still feel like it breaks our local-to-this-repo convention where `.range()` almost always returns the text range of a thing and never mutates a thing, even if it's a generally common pattern in Rust. But it doesn't matter that much.

---

_@MichaReiser reviewed on 2024-08-19 13:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:991 on 2024-08-19 13:23_

Fair enough. It just feels slightly more verbose because it now requires an extra `push_diagnostic` call, and this needs to be repeated at every call site. 

The overall idea is that the API is similar to `map.entry` where we want to defer the insertion and also already pre-set the required values (the key).  



---

_@AlexWaygood reviewed on 2024-08-19 13:24_

You also need to emit "unresolved import" diagnostics on `from foo import bar` imports where `foo` can be resolved to a module but there is no `bar` symbol inside the `foo` module. We do so on `main`, but this PR means that we now only emit "unresolved import" diagnostics on `import foo` imports or `from foo import bar` imports where the module `foo` cannot be resolved.

---

_Comment by @MichaReiser on 2024-08-19 13:25_

> You also need to emit "unresolved import" diagnostics on `from foo import bar` imports where `foo` can be resolved to a module but there is no `bar` symbol inside the `foo` module. We do so on `main`, but this PR means that we now only emit "unresolved import" diagnostics on `import foo` imports or `from foo import bar` imports where the module `foo` cannot be resolved.

Yeah, I'm not aiming for parity with the lint rule. The goal here is to add the API, and unresolved imports were the easiest example where I didn't have to make something up.


---

_Comment by @AlexWaygood on 2024-08-19 13:53_

> Yeah, I'm not aiming for parity with the lint rule. The goal here is to add the API, and unresolved imports were the easiest example where I didn't have to make something up.

Fair enough -- but parity with `main` isn't too hard here, and it also fixes the failing assertions you have currrently in the benchmarks. You just need to do this to your PR branch:

```diff
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -943,7 +943,7 @@ impl<'db> TypeInferenceBuilder<'db> {
             ModuleName::new(module_name)
         };
 
-        let module_ty = self.module_ty_from_name(module_name, import_from.into());
+        let module_ty = self.module_ty_from_name(module_name.clone(), import_from.into());
 
         let ast::Alias {
             range: _,
@@ -960,6 +960,15 @@ impl<'db> TypeInferenceBuilder<'db> {
             .member(self.db, &Name::new(&name.id))
             .replace_unbound_with(self.db, Type::Unknown);
 
+        // We'll already have emitted a diagnostic if `module_name` is `None`
+        if let Some(module_name) = module_name {
+            if ty.is_unknown() {
+                if let Some(builder) = self.push_diagnostic(alias.into(), "reportMissingImport") {
+                    builder.finish(format!("Could not resolve import of {name} from {module_name}"));
+                }
+            }
+        }
+
         self.types.definitions.insert(definition, ty);
     }
```

---

_@MichaReiser reviewed on 2024-08-19 14:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:46 on 2024-08-19 14:12_

Agree, but it also is the only reason why the type exists. It would just be a `Vec<TypeCheckDiagnostic>` if it wasn't for the `Arc` wrapping

---

_@MichaReiser reviewed on 2024-08-19 14:14_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:991 on 2024-08-19 14:14_

I simplified the API to use `format_args` to defer the expensive message allocation for now. We may need to go back to a builder-like pattern in the future. But that depends on how we design our diagnostics

---

_Comment by @MichaReiser on 2024-08-19 14:16_

> Fair enough -- but parity with main isn't too hard here, and it also fixes the failing assertions you have currrently in the benchmarks. You just need to do this to your PR branch:

I didn't do it because I am unclear on whether the diagnostic shouldn't be created in `ty.member`. That's why I still prefer to not make this change as part of this PR

---

_@AlexWaygood reviewed on 2024-08-19 14:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:46 on 2024-08-19 14:17_

Okay, but VSCode will display all this information on my screen if I'm hovering over the `TypeCheckDiagnostics` in another red-knot module. This feels like way more information than is necessary in that kind of situation :P

![image](https://github.com/user-attachments/assets/d3799e7a-7fee-4025-a9aa-b1139f6ed827)


---

_Comment by @AlexWaygood on 2024-08-19 14:19_

> I didn't do it because I am unclear on whether the diagnostic shouldn't be created in `ty.member`.

(Definitely not, because we'll also use `ty.member()` for e.g. `import foo; x = foo.bar` errors where the `foo` module has no member `bar` -- but there it's an unresolved attribute access rather than an unresolved import, so it'll need a different error message and a different code)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:989 on 2024-08-19 14:34_

```suggestion
                "unresolved-import",
```

---

_@AlexWaygood approved on 2024-08-19 14:37_

Thanks, I like this much more now. I'd still prefer it if we could have parity with `main` in this PR, but `¯\_(ツ)_/¯`

---

_Comment by @MichaReiser on 2024-08-19 14:38_

> (Definitely not, because we'll also use ty.member() for e.g. import foo; x = foo.bar errors where the foo module has no member bar -- but there it's an unresolved attribute access rather than an unresolved import, so it'll need a different error message and a different code)

Still, I don't know ;) Feels annoying having to test this on every `ty.member` call-site 

---

_Merged by @MichaReiser on 2024-08-20 07:22_

---

_Closed by @MichaReiser on 2024-08-20 07:22_

---

_Branch deleted on 2024-08-20 07:22_

---

_Comment by @codspeed-hq[bot] on 2024-08-20 07:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/type-check)

### Merging #12988 will **improve performances by 7.07%**

<sub>Comparing <code>type-check</code> (de62da1) with <code>main</code> (38c19fb)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `type-check` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 778.7 µs | 727.3 µs | +7.07% |


---
