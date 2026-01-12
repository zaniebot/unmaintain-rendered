```yaml
number: 14946
title: Bind top-most parent when importing nested module
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/module-resolution
created_at: 2024-12-12T20:37:25Z
updated_at: 2024-12-16T21:15:43Z
url: https://github.com/astral-sh/ruff/pull/14946
synced_at: 2026-01-12T15:55:49Z
```

# Bind top-most parent when importing nested module

---

_@dcreager_

When importing a nested module, we were correctly creating a binding for the top-most parent, but we were binding that to the nested module, not to that parent module.  Moreover, we weren't treating those submodules as members of their containing parents.  This PR addresses both issues, so that nested imports work as expected.

As discussed in ~Slack~ whatever chat app I find myself in these days :smile:, this requires keeping track of which modules have been imported within the current file, so that when we resolve member access on a module reference, we can see if that member has been imported as a submodule.  If so, we return the submodule reference immediately, instead of checking whether the parent module's definition defines the symbol.

This is currently done in a flow insensitive manner.  The `SemanticIndex` now tracks all of the modules that are imported (via `import`, not via `from...import`).  The member access logic mentioned above currently only considers module imports in the file containing the attribute expression.

---

_Comment by @dcreager on 2024-12-12 20:37_

(This is still marked as draft because I still plan on adding some additional test cases, but should be code complete if folks want to take an initial look at the implementation)

---

_Comment by @github-actions[bot] on 2024-12-12 20:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-12-13 07:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_name.rs`:203 on 2024-12-13 07:12_

Nit: I'd call this `push` similar to `PathBuf::push`, `Vec::push`, `String::push`, ...

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_name.rs`:227 on 2024-12-13 07:16_

Nit: I'd call this `ancestors` similar to `PathBuf::ancestors`. I'm further leaning towards naming it `ancestor_components` because the method returns a `&str`, unlike `ModuleName::Parent` that returns a `&str` or change the return type to `ModuleName` (or we introduce a `ModuleNameRef` if we're concerned about the potential allocation).

Nit2: I think we can simplify this by calling `ModuleName::parent` (Only true if you change the return type)

```
std::iter::successors(Some(self.clone()), Self::parent)
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:674 on 2024-12-13 07:17_

Nit:

* The usage suggest that we should change `parents` to return a `ModuleName`
* We can use `extend`
```suggestion
                        self.import_modules.extend(module_name.parents().map(|name| ModuleName::new(name).unwrap()))
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2662 on 2024-12-13 07:20_

Nit: I'm slightly leaning towards naming this `file`. It's the file in which the module is defined. Also I was a bit confused what `module_ref.module()` would return: The module of the module?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:82 on 2024-12-13 07:20_


```suggestion
            Type::ModuleLiteral(module) => {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3084 on 2024-12-13 07:25_

Nit: Could this logic be moved into `Type::member` (which then calls into `ModuleType::member`) or does it only apply when looking up attributes?



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2660 on 2024-12-13 07:28_

I don't have a good sense for what's better but we could remove the `name` here and instead call `file_to_module` and then retrieve the name from `module.name`. We can add a `name(db)` method that hides the `file_to_module` and `unwrap` calls. 

It would reduce the amount of data that we store, possibly at the cost that module attribute lookups are slightly slower.

---

_@MichaReiser reviewed on 2024-12-13 07:28_

This is great. I only have a few nit comments

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2662 on 2024-12-13 20:16_

Ah yes, this came from when I was also storing the file containing the reference that was originally resolved to this module, and in that case I was using more descriptive names to distinguish the two files.  But now I'm not, ~so I'll revert back to that name!~

Actually, with the change to store `Module` in here, `module` does (now) seem to be the right name.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/display.rs`:82 on 2024-12-13 20:17_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3084 on 2024-12-13 20:19_

I don't think it can be in `Type` because it requires resolving the module, which is currently handled by `TypeInferenceBuilder`.  And the "submodules are available as attributes of the containing package" bit is only relevant during attribute access.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/module_name.rs`:203 on 2024-12-13 20:20_

Yes, good catch!  I had my language wires crossed thinking of Python's `list.append`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/module_name.rs`:227 on 2024-12-13 20:24_

I like that `successors` formulation.  At least at the moment, the extra allocations aren't an issue since we were already doing a `clone` over in the semantic index builder.  So this change just moves where the allocation occurs.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:674 on 2024-12-13 20:25_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2660 on 2024-12-13 20:30_

Ah, actually this tells me we should just use `Module` here!  I've kept the `ModuleLiteralType` wrapper struct so that I could mark it as `salsa::tracked`, which lets `Type` still be `Copy`

---

_@dcreager reviewed on 2024-12-13 20:44_

---

_@dcreager reviewed on 2024-12-13 20:46_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3084 on 2024-12-13 20:46_

That said, resolving the module doesn't look like it _needs_ to be in `TypeInferenceBuilder`, since it's just calling `resolve_module`.  Let me see if I can move it into `Type` after all

---

_@dcreager reviewed on 2024-12-13 20:49_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3084 on 2024-12-13 20:49_

Sorry, no, scratch that — it needs access to the semantic index of the containing file to see which modules have been imported.  So the two options are:

- Leave it here
- Add the containing `File` to `ModuleLiteralType` so that we can move this logic into `Type::member`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3084 on 2024-12-13 21:43_

Latest patch adds the containing file to `ModuleLiteralType`.  Interestingly, that allowed one of the expected-failure mdtests to [start succeeding](https://github.com/astral-sh/ruff/pull/14946/commits/5fb1246cbe4dac1529defb990a8fb5c12c00b6a0#diff-5c8508e2bc14c029e901b266c723a52bbf1b574be8129942162b108492b77fcf)!



---

_@dcreager reviewed on 2024-12-13 21:43_

---

_Marked ready for review by @dcreager on 2024-12-13 21:49_

---

_Review requested from @carljm by @dcreager on 2024-12-13 21:49_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-13 21:49_

---

_Review requested from @sharkdp by @dcreager on 2024-12-13 21:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_name.rs`:203 on 2024-12-13 22:08_

Hmm, we work quite hard in the other methods of this struct to maintain the invariant that the wrapped string can only be a valid module name. I think this method breaks that invariant, since the `Name` node could wrap a string that constitutes an invalid identifier. (Remember that we have a parser that supports error recovery, so it can produce `Name` nodes that wrap invalid identifiers! There are few guarantees.)

I think to fix this we could:
- Change the signature of this method so that it accepts a `ModuleName` rather than a `Name` -- but then it has the same signature of `ModuleName::extend`.
- Verify that the string passed in is valid using `ModuleName::is_valid_name`, and change the return type of this method to return a `Result`, where an error variant is returned if you passed in a string that constitutes an invalid identifier. But then it becomes less ergonomic to use the method.

Overall I'm wondering if we need this new method, or if it might just be better to use the existing `ModuleName::extend()`?

---

_Comment by @carljm on 2024-12-13 22:23_

> As discussed in Slack

I doubt it 😆 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/tracking.md`:18 on 2024-12-13 22:26_

```suggestion
    track submodule imports flow-sensitively, in which case we will have to update the assertions in some of
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2814 on 2024-12-13 22:35_

Is this name clearer?

Also, doc comments to help clarify what these are?
```suggestion
    /// The file in which this module was imported.
    ///
    /// We need this in order to know which submodules should be attached to it as attributes
    /// (because the submodules were also imported in this file).
    pub importing_file: File,
    
    /// The imported module.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1445 on 2024-12-13 22:54_

I realize now reading this that the priority between a non-submodule package attribute and a submodule is a bit of a subtle thing.

What actually happens at runtime is that, when the submodule is imported, it writes its own name to the containing package namespace. This means that which takes priority actually depends on whether the package `__init__.py` itself imports the submodule before creating its own attribute of that same name, or not.

In other words, given `pkg/a.py` and these two versions of `pkg/__init__.py`:

```py
a = 1
```

```py
from . import a as _
a = 1
```

Then `import pkg.a; print(pkg.a)` will print `<module 'pkg.a' ...>` in the first case, but it will print `1` in the second case. Because in the first case, we first execute `pkg/__init__.py` fully, and only then import the submodule, overwriting the value `1` for the name `"a"` in the `pkg` namespace with the submodule. In the second case, we first import the submodule, which sets the submodule as the value for the `a` name, and then we continue executing `__init__.py`, overwriting the submodule with the value `1`.

Neither mypy nor pyright model this accurately. Pyright prefers the submodule in both scenarios, and mypy prefers `1` in both scenarios.

What you have here currently would match pyright.

Is it worth trying to model this accurately? I think probably not, at least not for now. But we could put some of this context into this comment? And/or we could add some tests for the above two cases, with commentary on how they behave at runtime vs in our model?

I think the behavior you have here currently is good for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2353 on 2024-12-13 23:07_

I realize now that what I had originally imagined we would do would be to look up the submodule data from `self.index` here and populate the interned `ModuleLiteralType` with the set of its imported submodule names. This means we would store more data per `ModuleLiteralType` (a set of submodule names, instead of just a File), but member lookups (in particular, those which don't match a submodule) would be a bit cheaper: the initial check would just be a membership test for the attribute name in the submodule set, without needing to build a `ModuleName`.

But I suspect all of this is just in the noise performance-wise (and the extra storage per `ModuleLiteralType` may well outweigh the cheaper lookups), so I don't see any reason to switch to that approach. Just thought the alternative worth mentioning.

---

_@carljm approved on 2024-12-13 23:10_

Lovely!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:671 on 2024-12-14 14:55_

nit: we implement `Deref<Target = str>` for the `ruff_python_ast::Identifier` struct

```suggestion
                    if let Some(module_name) = ModuleName::new(&alias.name) {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1449 on 2024-12-14 14:57_

what's the reason we wrap the string in an `ast::name::Name` node before passing it into `ModuleName::push`? `ModuleName::push` immediately converts it back to a `&str`, I think

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1456 on 2024-12-14 14:58_

nit: maybe we could create a new `Type::module_literal()` constructor for the `Type` enum, similar to our `Type::string_literal()` constructor and others?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2814 on 2024-12-14 15:01_

hmm, why do we need to store the `File` as a separate field on `ModuleLiteralType`, when the first field is a `Module`, and `Module` already stores the file?

The definition of `Module` is here -- it's just a thin wrapper around `ModuleInner`:

https://github.com/astral-sh/ruff/blob/c4df0a29ef81813b0f8274ba36064b60c0fbd265/crates/red_knot_python_semantic/src/module_resolver/module.rs#L9-L13

And `ModuleInner` is here:

https://github.com/astral-sh/ruff/blob/c4df0a29ef81813b0f8274ba36064b60c0fbd265/crates/red_knot_python_semantic/src/module_resolver/module.rs#L64-L70

The `Module` struct exposes a public getter method to retrieve the `File` of the module:

https://github.com/astral-sh/ruff/blob/c4df0a29ef81813b0f8274ba36064b60c0fbd265/crates/red_knot_python_semantic/src/module_resolver/module.rs#L37-L40

---

_@AlexWaygood reviewed on 2024-12-14 15:02_

Thanks!

---

_@carljm reviewed on 2024-12-14 15:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2814 on 2024-12-14 15:48_

It's not the same File. This is the file in which the import occurred, not the file of the module being imported. One of my comments recommends clarifying doc comments here. 

---

_@AlexWaygood reviewed on 2024-12-14 15:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2814 on 2024-12-14 15:50_

Ahh, thanks. I should have looked more closely -- my bad. Yes, I definitely support a better name for the field and more doc-comments, in that case! :-)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1449 on 2024-12-16 15:49_

Changed to use `extend` per other comment

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:671 on 2024-12-16 15:52_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1456 on 2024-12-16 15:55_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1445 on 2024-12-16 18:07_

Updated the comment and added a test case with commentary

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:2353 on 2024-12-16 18:13_

Yeah that's a good point that this would make us construct a `ModuleName` every time we dereference a module.  I had considered a more complex representation for `ModuleName`, but figured it would be better to experiment with that separately instead of trying to fold it into this.  (Or better to just say this is good enough and leave it alone for now)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/module_name.rs`:203 on 2024-12-16 18:28_

Yes that's a great point — I had gotten it into my mind somehow that `Name` was similarly checked, and so appending one would be safe just like extending a `ModuleName` is.  But it's not, and even if we added one, I believe we have to handle `__getattr__` calls here, too, which aren't guaranteed to be valid identifiers.

Updated this to use `extend` and to check the result of `ModuleName::new`

---

_@dcreager reviewed on 2024-12-16 18:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1448 on 2024-12-16 18:38_

Hmm, I'm a bit concerned about calling `semantic_index` here. Are there cases where `importing_file != self.file`? If so, then we shouldn't call `semantic_index` here because it would introduce a cross-file dependency: This file would always be rechecked if the `importing_file`'s AST changes.



---

_@MichaReiser reviewed on 2024-12-16 18:38_

---

_@MichaReiser reviewed on 2024-12-16 18:39_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2897 on 2024-12-16 18:39_

Does that mean we have multiple `ModuleLiteralType`s for the same module? One for each place from which it's imported?

---

_@MichaReiser reviewed on 2024-12-16 18:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1448 on 2024-12-16 18:41_

Never mind. I thought this is in the `SemanticindexBuilder` but that's not true. So I think this is fine. 

---

_@MichaReiser reviewed on 2024-12-16 18:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1455 on 2024-12-16 18:45_

This is more of a fine-tuning comment: 
It could make sense to define an `imported_modules(db, file)` query that returns the set with all known imported modules (it would just be a thin wrapper around `semantic_index(db, file).imported_modules()`. 

The advantage of having this intermediate query is that currenlty, `Type::member` has to re-execute whenever the AST of `importing-file` changes. Salsa might to truncate any dependent queries because it can determine that the type returned by `Type::member` is still the same and, therefore, dependent queries don't have to re-run. However, salsa could determine this sooner if `Type::member` only dependent on a `imported_modules` query. It would then only have to re-run the `imported_modules` query and, if the imports are unchanged, can't stop immediately. 

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2897 on 2024-12-16 19:47_

Yes that's right

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1455 on 2024-12-16 19:48_

Done, thanks for the suggestion!  I wrapped the `imported_modules` set in an `Arc`, since it seems like the set now needs to be able to outlive the containing `SemanticIndex`, is that right?

---

_@dcreager reviewed on 2024-12-16 19:48_

---

_@carljm reviewed on 2024-12-16 20:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2353 on 2024-12-16 20:01_

I think that unless we a) are seeing a clear regression in the existing benchmark, or b) there's a very clear alternative that will definitely perform better, it's best not to spend much time on performance optimizations when we don't even know if they are meaningful in the overall performance profile. So I would suggest leaving this as-is for now.

---

_@carljm approved on 2024-12-16 20:10_

This looks merge-ready to me!

---

_Merged by @dcreager on 2024-12-16 21:15_

---

_Closed by @dcreager on 2024-12-16 21:15_

---

_Branch deleted on 2024-12-16 21:15_

---
