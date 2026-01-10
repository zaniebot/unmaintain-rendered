```yaml
number: 12141
title: "[red-knot] Respect typeshed's `VERSIONS` file when resolving stdlib modules"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: custom-typeshed-versions
created_at: 2024-07-01T13:47:40Z
updated_at: 2024-07-05T22:57:14Z
url: https://github.com/astral-sh/ruff/pull/12141
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Respect typeshed's `VERSIONS` file when resolving stdlib modules

---

_Pull request opened by @AlexWaygood on 2024-07-01 13:47_

## Summary

In the module resolver, we need to treat standard-library paths differently to paths relative to other module-resolver search paths. This is for three reasons:
- Unlike for other module-resolver paths, only `.pyi` files are valid relative to standard-library paths
- When checking whether standard-library paths "exist", we need to take account of typeshed's `VERSIONS` file as well as simply checking whether the path exists on disk.
- Although this isn't implemented now, we will also at some point need to implement support for vendored standard-library files as well as standard-library files in a custom typeshed directory that actually exists on disk.

Although it's not implemented here yet, two other kinds of paths will also need special treatment in the module resolver:
- The module resolver needs to treat `site-packages` search paths differently, due to the fact that a module `foo` could resolve to a `site-packages/foo` path or a `site-packages/foo-stubs/foo` path
- If we decide to vendor third-party stubs from typeshed as well as standard-library stubs, these will need to be treated specially as well.

To account for these differences, this PR introduces a new module, `crates/red_knot_module_resolver/src/path.rs`. The new module has two enums,  `ModuleResolutionPathBuf` and `ModuleResolutionPathRef`. Each enum has four different variants representing the four different kinds of search paths the module resolver must handle:
- `ExtraPath`: paths representing Step (1) in the module resolution order at https://typing.readthedocs.io/en/latest/spec/distributing.html#import-resolution-ordering
- `FirstPartyPath`: paths representing first-party user code
- `StandardLibraryPath`: paths representing standard-library stubs from typeshed
- `SitePackagesPath`: paths representing steps (4) and (5) in the module resolution order at https://typing.readthedocs.io/en/latest/spec/distributing.html#import-resolution-ordering

The different variants abstract over the fact that for different kinds of paths, there will be different answers to questions such as "Is this a valid `join()` operation?", "Does this path exist?" and "Does this path represent a directory?". 

## Test Plan

Unit tests have been added to `src/path.rs` and `src/typeshed/versions.rs`. Integration tests have been added to `src/resolver.rs`.


---

_Label `red-knot` added by @AlexWaygood on 2024-07-01 13:47_

---

_Comment by @github-actions[bot] on 2024-07-01 14:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/db.rs`:22 on 2024-07-01 14:18_

I think I would make a `typeshed_versions(db)` query instead. It should be possible to have zero-argument queries.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/module.rs`:23 on 2024-07-01 14:19_

Nit: The name `Entry` feels a bit odd there. Why not just `ModuleSearchPath`?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:98 on 2024-07-01 14:21_

Nit: Just `exact`. `get` prefixes are uncommon in the Rust ecosystem
```suggestion
    fn exact(&self, module_name: &ModuleName) -> Option<&PyVersionRange> {
        self.0.get(module_name)
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:85 on 2024-07-01 14:22_

What's the reason for supporting `Clone`? Seems expensive

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/db.rs`:65 on 2024-07-01 14:26_

Nit: Maybe add a `Default` or `empty` function to avoid this spreading everywhere

---

_@MichaReiser reviewed on 2024-07-01 14:40_

I feel a bit mixed about having all those different `Path` versions. It feels like the right level of abstraction but it's a lot of code and it becomes hard to spot how they are different because of all the boilerplate that is identical between all paths. I think we should try to reduce the amount of code. I'm not entirely sure yet how, but I think we should try

I think a simpler design would be to just have an enum with different variants. Very similar to what we have with `ModuleSearchPath`. I think it would help readability because I can then see in the methods how the different path variants differ.

One thing that I think would already help if we wouldn't need both `Path` and `PathBuf` variants. Are there many places where we use `Path` where it helps to avoid allocating and where we can't use a `&PathBuf`? 

It seems that `ModuleSearchPath` implements a s similar interface to each `Path` variant. 

---

_Comment by @AlexWaygood on 2024-07-01 14:54_

Thanks, agree that it's a lot of code (more than I thought it would be, and more than I'm comfortable with). I'll try to address that.

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:85 on 2024-07-01 15:20_

It's required for snapshotting. I'll put the `FxHashMap` behind an `Arc` to make it less expensive to clone. It makes sense for the inner mapping to be immutable: if the user changes the `VERSIONS` file, we'll need to reparse the whole thing anyway.

---

_@AlexWaygood reviewed on 2024-07-01 15:20_

---

_@AlexWaygood reviewed on 2024-07-02 11:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:169 on 2024-07-02 11:19_

Unwrapping isn't correct here; if the paths fail to validate, we need to bail out from the whole process and tell the user that we were passed an invalid path in the configuration they gave us. Will do that soon.

---

_Comment by @AlexWaygood on 2024-07-02 11:23_

https://github.com/astral-sh/ruff/pull/12141/commits/d91626c5301ffb9e79653cf5afb6592c5f0ce8c2 made this PR around 600 lines of code shorter! The `path.rs` file looks pretty different now 🥳

---

_@AlexWaygood reviewed on 2024-07-02 18:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:18 on 2024-07-02 18:45_

Unwrapping is obviously the wrong thing to do here... Should I just return a `Result` from this query? Not sure how to handle this. If the user passes us a custom typeshed directory with an invalid `VERSIONS` file, we can't infer any information about the standard library

---

_Comment by @AlexWaygood on 2024-07-02 19:00_

This still needs a lot more tests, but other than that it's basically where I want it to be now... though there's still a bunch of `.unwrap()` calls where I should be bubbling up errors instead.

---

_Comment by @MichaReiser on 2024-07-03 06:49_

> This still needs a lot more tests, but other than that it's basically where I want it to be now... though there's still a bunch of .unwrap() calls where I should be bubbling up errors instead.

Handling and propagating errors is something that I have yet to fully figure out for Red Knot. 

Generally, the idea is that query methods shouldn't fail and instead keep doing something useful to make ruff more error resilient and it simplifies consuming queries (it would be rather annoying if `source_text` returned a `Result` because all dependent queries would have to return a `Result` as well). 

I could see this working for `source_text` where reading a file failed. ' source_text pushes a diagnostic using a Salsa accumulator, returns an empty string, and stores a flag on the result that the read operation failed. For most queries calling `source_text`, checking the error isn't necessary, an empty string works just fine for them. However, we probably want to check the error flag when calling `check_file` or `format` or any other "file-level" operation to short-circuit in this case. 

I see two options for how we can handle this with versions:

* If reading `versions` fails (or the custom typeshed directory doesn't exist), emit a diagnostic and fall back to the vendored stubs.
* It's still unclear to me if we handle settings as part of queries or if settings are discovered ahead of time when discovering all the workspace files. If that's the case, then parsing and resolving the settings would happen outside salsa where we could handle the error appropriately. 

For now, I think adding a TODO and/or opening an issue to follow up with error handling seems fine to me. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:41 on 2024-07-03 07:06_

To me the benefit of having all these different `Path` variants isn't entirely clear. I get the impression that we now use two forms to distinguish the paths:

* The variant in `ModuleResolutionPath` 
* The different path types

The only place where I see that we pass a specific path to a method is `StandardLibraryPath`. Considering that this method is private, I think it's fine to just pass `FileSystemPath`. 

I would recommend removing the different path versions and just use `FileSystemPathBuf` everywhere and solely rely on the enum variant to distinguish the different cases.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:578 on 2024-07-03 07:09_

Do we rely on this logic a lot. I'm worried that this will bite us when adding support for vendored file system because implementing `AsRef` then becomes impossible.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:635 on 2024-07-03 07:11_

The `Eq` implementation here are unused and I'm not quiet sure if an equal implementation that doesn't take the module search path variant into account matches what I would expect. 

Let's also remove all other trait implementations that are currently unused. We can always add them once needed.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:410 on 2024-07-03 07:15_

Not a huge deal, but the method is non trivial and the `impl Into<Self>` means that this method will get monomorphized for each type; You can avoid monomorphization by creating an inline function
```suggestion
    pub(crate) fn is_regular_package(&self, db: &dyn Db, search_path: impl Into<Self>) -> bool {
    		fn is_regular_impl(path: &ModuleResolutionSearchPath, db: &dyn Db, search_path) -> bool {
	        match (path, search_path) {
	            (Self::Extra(ExtraPath(fs_path)), Self::Extra(_))
	            | (Self::FirstParty(FirstPartyPath(fs_path)), Self::FirstParty(_))
	            | (Self::SitePackages(SitePackagesPath(fs_path)), Self::SitePackages(_)) => {
	                let file_system = db.file_system();
	                file_system.exists(&fs_path.join("__init__.py"))
	                    || file_system.exists(&fs_path.join("__init__.pyi"))
	            }
	            // Unlike the other variants:
	            // (1) Account for VERSIONS
	            // (2) Only test for `__init__.pyi`, not `__init__.py`
	            (Self::StandardLibrary(StandardLibraryPath(fs_path)), Self::StandardLibrary(stdlib_root)) => {
	                let Some(module_name) = self.as_module_name() else {
	                    return false;
	                };
	                let typeshed_versions = Self::load_typeshed_versions(db, stdlib_root);
	                match typeshed_versions.query_module(&module_name, get_target_py_version(db)) {
	                    TypeshedVersionsQueryResult::Exists
	                    | TypeshedVersionsQueryResult::MaybeExists => {
	                        db.file_system().exists(&fs_path.join("__init__.pyi"))
	                    }
	                    TypeshedVersionsQueryResult::DoesNotExist => false,
	                }
	            }
	            (path, root) => unreachable!(
	                "The search path should always be the same variant as `self` (got: {path:?}, {root:?})"
	            )
	        }
	     }
	     is_regular_impl(self, db, search_path.into())    
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:51 on 2024-07-03 07:16_

I think I would call this enum `ModuleResolutionPathBuf` and the other `ModuleResolutionPath`

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:472 on 2024-07-03 07:20_

Nit: I think we could move this to `ModuleName::from_components` where the argument is `impl IntoIterator<Item=&str>`



---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:691 on 2024-07-03 07:22_

This is nead but doesn't seem needed at the moment. I recommend deleting it.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:650 on 2024-07-03 07:23_

Nit: I think you can avoid the option here
```suggestion
    parent_components: Option<camino::Utf8Components<'a>>,
    stem: Option<&'a str>,
}

impl<'a> ModulePartIterator<'a> {
    #[must_use]
    fn from_fs_path(path: &'a FileSystemPath) -> Self {
        let mut parent_components = path.components();
        parent_components.next_back();
        Self {
            parent_components,
            stem: path.file_stem(),
        }
    }
}
```

This should also simplify the `next` implementation

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:453 on 2024-07-03 07:25_

The Rust convention is to call this `to_module_name` because the conversation isn't "free". See [naming guidelines](https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:439 on 2024-07-03 07:28_

Nit: I would be calling this `strip_dunder_init` to align with `str::strip_sufix` (strip is probably also a more familiar word to none native English or French speakers)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:68 on 2024-07-03 07:32_

```suggestion
            if let Some(extension) = camino::Utf8Path::new(component).extension() {
```

since you already depend on `Utf8Path` anyway and it avoids having to call `to_str` to deal with non unicode paths

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:356 on 2024-07-03 07:36_

I'm leaning towards removing all methods that take a `&db` out of path and instead implement them as methods on `ModuleResolver`. It just feels a bit heavyweight to me that an `is_directory` call ends up loading the type shed versions. It also makes it harder to optimize caching of typeshed versions; e.g. the module resolver could load the typeshed versions once and cache them for the entire resolution of a `resolve_module` call where path needs to call `parse_typeshed_versions` for every `is_directory`, `is_regular_package`, ...  call

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:131 on 2024-07-03 07:37_

I wonder if we should change the settings to `Option<(FileSystemPathBuf, TypeshedVersions)>`. This way, parsing the typeshed versions happens outside of salsa (solves the error reporting problem). 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:128 on 2024-07-03 07:40_

Nit: I'm inclined to rename `SupportedPyVersion` to `TargetPyVersion` considering that you named the field very consistently `target_version`

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:169 on 2024-07-03 07:42_

I think this is where a builder for `ModuleSearchSettings` could help where `ModuleSearchSettings` stores `ModuleResolutionPath` internally but the builder exposes `with_extra(extras) -> Result<Self, Self>` methods. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:15 on 2024-07-03 07:44_

You can use `return_ref` to avoid the need for an `Arc`
```suggestion
#[salsa::tracked(return_ref)]
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:115 on 2024-07-03 07:45_

Nit: Can this method be `pub(crate)` (and maybe some other types and methods?)

---

_@MichaReiser approved on 2024-07-03 07:49_

This is looking good!

I think we can simplify it a bit further and I would move some code around but we're on a good path here!

---

_Review requested from @MichaReiser by @MichaReiser on 2024-07-03 07:50_

---

_@AlexWaygood reviewed on 2024-07-03 08:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:131 on 2024-07-03 08:21_

Hmm, but we need to re-parse the `VERSIONS` file every time it changes if we're in an LSP context. I think a Salsa query is pretty well suited to this, if I understand correctly. The difficulty just comes in bubbling the error out of salsa

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:131 on 2024-07-03 08:27_

But the need for recomputing also applies to the `custom_typeshed` path? 

Salsa will re-run all `resolve_module` queries when the `ModuleResolutionSettings` change. The only thing that happens outside (at least for now) is how we react to setting changes.

---

_@MichaReiser reviewed on 2024-07-03 08:27_

---

_@AlexWaygood reviewed on 2024-07-03 08:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:131 on 2024-07-03 08:33_

The user won't pass a direct path to the VERSIONS file on the command line or in a config file. They'll pass a direct path to the custom typeshed directory, and the VERSIONS file is found inside that directory. If they then edit the contents of the VERSIONS file (or delete it entirely), we'll need to react to that change and attempt to re-parse the VERSIONS file in the custom typeshed directory, even though none of the settings passed by the user will have changed in any way

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:15 on 2024-07-03 09:25_

Ah nice, this means I also don't need to derive `Clone` at all for `TypeshedVersions`! I didn't like that at all.

---

_@AlexWaygood reviewed on 2024-07-03 09:25_

---

_@AlexWaygood reviewed on 2024-07-03 09:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:453 on 2024-07-03 09:37_

I tried making this change, but Clippy complained:

```
warning: methods with the following characteristics: (`to_*` and `self` type is `Copy`) usually take `self` by value
   --> crates/red_knot_module_resolver/src/path.rs:406:34
    |
406 |     pub(crate) fn to_module_name(&self) -> Option<ModuleName> {
    |                                  ^^^^^
    |
    = help: consider choosing a less ambiguous name
    = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#wrong_self_convention
    = note: `#[warn(clippy::wrong_self_convention)]` on by default
```

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:439 on 2024-07-03 09:39_

Pardon, monsieur

---

_@AlexWaygood reviewed on 2024-07-03 09:39_

---

_@AlexWaygood reviewed on 2024-07-03 09:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:650 on 2024-07-03 09:44_

Hmm, not sure if that works or not. Let me write some tests for this bit of code before applying this change.

---

_@AlexWaygood reviewed on 2024-07-03 09:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:51 on 2024-07-03 09:52_

Hmm, I made this change, but now I'm wondering if that naming scheme is somewhat confusing. `FooPath` structs generally always need to be behind `&` references when you use them, because they're unsized types. But it's impossible to have an unsized enum (unfortunately), so the enum that represents the borrowed version of the path cannot be an unsized type; instead, it holds references internally.

---

_@AlexWaygood reviewed on 2024-07-03 10:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:51 on 2024-07-03 10:01_

The owned enum is now called `ModuleResolutionPathBuf`, as per your suggestion, but the one that has borrowed variants I've renamed back to `ModuleResolutionPathRef`

---

_@AlexWaygood reviewed on 2024-07-03 10:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:128 on 2024-07-03 10:12_

I see the logic there, but I already have a struct called `TargetPyVersion` in `supported_py_version.rs`. What do I call that if I give that name to the enum currently called `SupportedPyVersion`?

---

_@AlexWaygood reviewed on 2024-07-03 10:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:691 on 2024-07-03 10:14_

:( Then we have to have the default implementation for `last()`, which exhausts the entire iterator to get the last element

---

_@AlexWaygood reviewed on 2024-07-03 11:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:356 on 2024-07-03 11:21_

I see your point. However, the advantage of doing this as late as possible is that we only parse the typeshed versions when we actually need to: some module resolutions won't require us to look at typeshed `VERSIONS` at all, whereas if the method requires `TypeshedVersions` as an argument then we need to parse the `VERSIONS` file eagerly whether or not it ends up being resolved using a standard-library search path. `parse_typeshed_versions` is a Salsa-tracked function, as well, so the results should be memoized after the first call until the `VERSIONS` file is edited or deleted by the user from the custom typeshed directory (which should be very rare).

---

_@MichaReiser reviewed on 2024-07-03 13:24_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:691 on 2024-07-03 13:24_

Oh, I wasn't aware that we use `last` somewhere.

---

_@MichaReiser reviewed on 2024-07-03 13:26_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:453 on 2024-07-03 13:26_

This is because your type is copy, so you have to change from `&self` to `self`

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:453 on 2024-07-03 13:27_

Aha, thanks

---

_@AlexWaygood reviewed on 2024-07-03 13:27_

---

_@AlexWaygood reviewed on 2024-07-03 13:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:691 on 2024-07-03 13:41_

We don't. It just feels like a theoretical footgun to provide an `Iterator` interface that _could_ have a very efficient `last()` method, but doesn't.

---

_@MichaReiser reviewed on 2024-07-03 13:44_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:691 on 2024-07-03 13:44_

I would remove it then. By default, calling `last` on an `Iterator` is `O(n)`. That's why I always double check if the iterator implements `DoubleEndedIterator` if I call the method in a hot code path.

---

_@AlexWaygood reviewed on 2024-07-03 14:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:41 on 2024-07-03 14:10_

I'm not sure what the best long-term solution here is. In the long term, we may want to go back to this, as there are complications to do with `site-packages` that I haven't yet implemented. However, I agree that for now, this doesn't really buy us much. I've got rid of the inner structs.

---

_@AlexWaygood reviewed on 2024-07-03 14:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:578 on 2024-07-03 14:11_

I've just made a number of details more private to `path.rs`, so that we don't rely on this logic nearly as much. I think it should be fairly easy to adjust it when we add support for vendored files.

---

_@AlexWaygood reviewed on 2024-07-03 14:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:691 on 2024-07-03 14:11_

Alright :(

---

_@AlexWaygood reviewed on 2024-07-03 15:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:650 on 2024-07-03 15:05_

You're right, this does do the right thing. The thing I was confused about is for some reason I wasn't sure whether `next_back()` calls consumed the end of the iterator in a way that would be guaranteed to then be reflected when walking the iterator from the start. But it makes sense that it would be.

---

_@AlexWaygood reviewed on 2024-07-04 14:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:169 on 2024-07-04 14:10_

Possibly, but I'm still not sure I like that. I'd prefer to defer that to a followup, anyway. I'll add a TODO here.

---

_Marked ready for review by @AlexWaygood on 2024-07-04 16:45_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-04 16:45_

---

_Comment by @AlexWaygood on 2024-07-04 16:47_

Okay -- I'm now happy with the test coverage here! This is out of draft mode now, and ready for review.

Again -- let me know if you would prefer me to split this up a bit. It would be pretty easy to do so, if that would make reviewing easier. I don't mind.

---

_Renamed from "[red-knot] WIP: respect typeshed's `VERSIONS` file when resolving stdlib modules" to "[red-knot] Respect typeshed's `VERSIONS` file when resolving stdlib modules" by @AlexWaygood on 2024-07-04 16:55_

---

_@AlexWaygood reviewed on 2024-07-04 16:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/module_name.rs`:1 on 2024-07-04 16:57_

(This is nearly all just directly moved from `module.rs`, so that it can be imported in `path.rs`)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:169 on 2024-07-04 16:59_

Seems fair. Can we add a `TODO` to all call sites where we do `unwrap`?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:946 on 2024-07-04 17:01_

Can you try removing the `*` on both sides? I get the impression that both paths are probably already of the same type and we can just compare the references.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:121 on 2024-07-04 17:02_

You could move the test methods into mod tests

```
#[cfg(test)]
mod tests {
	impl TypeshedVersions {
		fn len(&self) -> size { }

		...
}
```

It would remove the need for the comment.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:161 on 2024-07-04 17:03_

Would you mind documenting the variants. To me it's not very clear what `MaybeExists` means

---

_Comment by @AlexWaygood on 2024-07-04 17:03_

I appreciate that it's still a bit light on docs. I can work on some more internal documentation for some of the added types tomorrow.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:123 on 2024-07-04 17:06_

I recommend removing this code. It only gets outdated and doesn't convey a lot of information. On the other hand, I would find a comment explaining what the function does useful

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:128 on 2024-07-04 17:07_

Do we have many places where we pass something that isn't a `PyVersion`? I would otherwise recommend just taking a `PyVersion` to avoid any monomorphization (every `impl` is a tradeoff between calling convenience and code size)

---

_@AlexWaygood reviewed on 2024-07-04 17:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:578 on 2024-07-04 17:15_

While writing tests, I realised that the two places where we still used this... were in fact bugs. So it's gone entirely now 😄

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:356 on 2024-07-05 06:25_

I agree, we should avoid resolving `TypeshedVersions` if they aren't needed. I think you can use an `Option` or `OnceCell` to cache the versions lazily. https://doc.rust-lang.org/std/cell/struct.OnceCell.html

> parse_typeshed_versions is a Salsa-tracked function, 

That's correct, but caching still has a non-trivial overhead of doing a lookup in a concurrent hash map and then checking if the revision is still fresh. 




---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:131 on 2024-07-05 06:27_

That's a good point. I'm okay with delaying it. I do think that we want some form of eager validation of the typeshed path and possibly even abort if the path is invalid. But we can look into this when adding support for settings. 

Having said that. I would expect that the CLI eagerly validates the custom typeshed path and aborts if the path doesn't exist or the versions file can't be parsed.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:128 on 2024-07-05 06:28_

I think for now I would make `TargetPyVersion` a field on `ModuleResolutionSettings`. I expect that the field (and all other module resolution settings) will be derived from the settings long-term. It also simplifies the setup because we only need a single "Input" for everything module resolution later (and it solves your naming problem)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:290 on 2024-07-05 06:34_

The logic here is almost the same as in `is_regular_package` except that it doesn't append `__init__.pyi` to the file path. Is this intentional? If not, can we extract the common logic into a standalone function? Same for `is_directory`, they all seem to be almost the same. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:468 on 2024-07-05 06:36_

Is it possible that `strip_prefix` can create a path which violates the constraints that `Self::first_party` requires? If not, then use
```suggestion
            ModuleResolutionPathRefInner::FirstParty(root) => absolute_path
                .strip_prefix(root)
                .ok()
                .map(|paht| Self(ModuleResolutionPathInner::FirstParty(path))),
            ModuleResolutionPathRefInner::StandardLibrary(root) => absolute_path
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:507 on 2024-07-05 06:37_

The `from` implementation here only seems to be necessary to implement `impl<'a> From<&'a ModuleResolutionPathBuf> for ModuleResolutionPathRef<'a> {`

I would remove this impl and instead move the code right into `impl<'a> From<&'a ModuleResolutionPathBuf> for ModuleResolutionPathRef<'a> {`

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:161 on 2024-07-05 06:38_

This implementation is unused. We should delete it

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:537 on 2024-07-05 06:43_

This `Eq` implementation seems unused. Let's delete it

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:549 on 2024-07-05 06:43_

You don't need the explicit lifetimes
```suggestion
impl PartialEq<FileSystemPath> for ModuleResolutionPathRef<'_> {
    fn eq(&self, other: &FileSystemPath) -> bool {
        self.0.as_file_system_path() == other
    }
}

impl PartialEq<ModuleResolutionPathRef<'_>> for FileSystemPath {
    fn eq(&self, other: &ModuleResolutionPathRef) -> bool {
        self == other.0.as_file_system_path()
    }
}

```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:567 on 2024-07-05 06:44_

I recomment using `assert_eq` here. It gives better error messages in case the path is `Some`
```suggestion
        assert_eq!(ModuleResolutionPathBuf::standard_library("foo.py"), None);
        assert_eq!(ModuleResolutionPathBuf::standard_library("foo/__init__.py"), None);
        assert_eq!(ModuleResolutionPathBuf::standard_library("foo.py.pyi"), None);
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:581 on 2024-07-05 06:48_

I don't think snapshot testing is a good choise for these tests. In general, consider snapshot testing a last resort. I know, snapshot tests are very convenient to write but they come up at a cost:

* They aren't very explicit about what they are asserting. What is this test asserting: That the path is a `StandardLibrary` path or that the path is `foo` or both? 
* They are easy to update but changes are hard to review (because the assertions aren't explicit). The chances that I miss a true-positive test failure in a snapshot is a multitude higher than for a regular test. 
* Any changes to the debug output of `ModuleResolutionPath` breaks all tests. 
* Snapshot testing is a heavy infrastructure, making our tests slower

I would stronlgy prefer to use regular assertions instead. Here, it's as easy as using `assert_eq(stdlib_path_test_case("foo"), ModuleResolutionPathRef(ModuleResolutionPathInner::StandardLibrary(FileSystemPath::new("foo")));

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:576 on 2024-07-05 06:49_

The name here should either be `ModuleResolutionPathRef` or `ModuleResolutionPathBuf`, because no type `ModuleResolutionPath` exists.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:852 on 2024-07-05 06:50_

IMO: If the assertion is worth testing, than it shouldn't be a runtime assertion OR the constructor should return `None` in that case. 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:954 on 2024-07-05 06:52_

We should make sure that the debug implementations between `ModuleResolutionPathRef` and `ModuleResolutionPathBuf` are consistent (by either removing the custom Debug implementation or have a custom debug implementation for both). 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:488 on 2024-07-05 06:57_

This test now seems to do way more than just asserting whether the file can be parsed and I find it hard to understand. It also depends on the real `VERSIONS` file, meaning a test failure here can either mean that the `VERSIONS` file has changed or that the implementation broke. 

I would strongly prefer to split this test into smaller tests where each uses a trimmed down `VERSIONS` file (ideally, only the relevant lines for that test). Splitting this test into smaller tests will help readers to understand the test-scenario and expected output. For exmaple, to me it's unclear why we need this many tests? Are we trying to exhaustively test all possible versions or is each of these blocks testing a specific scenario?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:27 on 2024-07-05 07:00_

We should try to avoid creating unnecessary salsa ingredients. Querying them, persisting them, and keeping them in memory all comes at a cost. I think I would aim for a single input ingredient for everything module resolver related (which kind of comes back that `ModuleResolutionSettings` maybe should be that input and it gets created with a builder)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/db.rs`:226 on 2024-07-05 07:04_

Nit
```suggestion
        let src = FileSystemPath::from("/src");
        let site_packages = FileSystemPath::from("/site_packages");
        let custom_typeshed = FileSystemPath::from("/typeshed");
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/db.rs`:232 on 2024-07-05 07:05_


```suggestion
        fs.create_directory_all(&src)?;
        fs.create_directory_all(&site_packages)?;
        fs.create_directory_all(&custom_typeshed)?;
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/module_name.rs`:1 on 2024-07-05 07:08_

What's the **not** nearly part :D

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/module_name.rs`:172 on 2024-07-05 07:11_

IMO: The nesting here makes the code rather hard to read. 
```suggestion
        let name = if let Some(second_part) = components.next() {
            if !is_identifier(second_part) {
                return None;
            }
            let mut name = format!("{first_part}.{second_part}");
            for part in components {
                if !is_identifier(part) {
                    return None;
                }
                name.push('.');
                name.push_str(part);
            }
            CompactString::from(&name)
        } else {
            CompactString::from(first_part)
        };

        Some(Self(name))
```

But I think we can simplify the implementation to 

```rust
        let mut components = components.into_iter();
        let first_part = components.next()?;
        if !is_identifier(first_part) {
            return None;
        }

        let mut name = first_part.to_compact_string();
        for part in components {
            if !is_identifier(part) {
                return None;
            }

            name.push('.');
            name.push_str(part);
        }

        Some(Self(name))
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:169 on 2024-07-05 07:15_

Nit: 

```
let mut paths: Vec<_> = extra_paths
            .into_iter()
            .map(|path| ModuleResolutionPathBuf::extra(path).unwrap())
            .collect();
```

Makes it more obvious which path failed to unwrap.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:378 on 2024-07-05 07:19_


```suggestion
            .write_file(&foo_path, "print('Hello, world!')")?;
```



---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:391 on 2024-07-05 07:19_


```suggestion
        assert_eq!(&foo_path, foo_module.file().path(&db));
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:388 on 2024-07-05 07:22_

You can add one more `PartialEq` implementation to avoid all the stars here

```rust
impl PartialEq<ModuleResolutionPathRef<'_>> for &FileSystemPath {
    fn eq(&self, other: &ModuleResolutionPathRef) -> bool {
        *self == other
    }
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:373 on 2024-07-05 07:23_

Nit: Maybe consider adding a simple `setup_resolver_test()` function that calls `create_resolver_builder().build()` to make the tests a bit less verbose.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:510 on 2024-07-05 07:24_

I think I would move this into its own test. The boilerplate is fairly limited and it is testing something else.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:482 on 2024-07-05 07:25_

What's the difference between this test and the `py38` test other than the python version. Would just one of the tests be sufficient or are both needed to test the full behavior? 

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:539 on 2024-07-05 07:26_

I recommend you to go once more througth all `*` usages and see if you can use `&` instead or don't need them all together.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:296 on 2024-07-05 07:28_

Nice, that's even better than a custom iterator!

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:583 on 2024-07-05 07:39_

Wow, this is very intensive testing that you added for path and I really apprevciate your care and time that you took to write all of them. 

I'm feeling a bit conflicted about some of the tests. I can see how they're useful when making the implementation but I fear they'll create a larger long-term coast than the value they provide (and writing the tests also comes at a cost):

* A lot of these (not all) tests test simple trivia. I can see that having the test can help when implementing the function but I'm not sure what the long-term value of the tests are and maintaining the code comes at a cost (and just looking at 1500 lines of tests is intimiating, and hides the real important tests).
* Most of the tests are precisely testing the implementation (and often match the implementation 1:1). Any change to the implementation will require a change to the test, significantely reducing the value of the tests and increasing maintenance cost long term. The most valuable tests are tests that keep functioning even when you refactor code. I think the resolver tests are a good exmaple. We refactored the implementation 2 times but the tests are still mostly unchanged, providing a safety net when making the refactor. I don't this applies to some tests in this module. Refactoring the path representation would require rewriting all these tests.

This makes me wonder how many of these tests we still need and if we could get the same coverage by extending our module resolver tests (e.g. by porting the tests from import resolver). I think we should at least reduce the number of tests. We don't need to aim for full exhaustiveness when testing. 
For example, do we need all of the `stdlib_path_*` tests or would one where it returns `None` and one where it returns `Some` be sufficient? I do like the `module_name_*` but do we need all of them? Are all of them testing different code paths or are we trying to test for full exhaustiveness? 


We don't need to do this as part of this PR, the PR is already big enough. But increasing the module resolver coverage seems desirable to me, especially if it helps to avoid some more "boilerplate"ish tests in here.


---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:656 on 2024-07-05 07:43_

Uh, I don't think I understand what this method is doing. I think it tests that `to_module_name` of each variant returns the same module name and it then returns one of the module names?

I think writing out the assertions would make debugging this test much easier (and there are only three variants!). I would also take the expected module name as an argument, again, simplifying the code


```suggestion
    fn assert_non_stdlib_module_name(path: &str, expected_name: &ModuleName) {    
        assert_eq!(&ModuleResolutionPathRef::extra(path).unwrap().to_module_name(), expected_module_name);
        assert_eq!(&ModuleResolutionPathRef::first_party(path).unwrap().to_module_name(), expected_module_name);
        assert_eq!(&ModuleResolutionPathRef::site_packages(path).unwrap().to_module_name(), expected_module_name);
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:572 on 2024-07-05 07:45_

IMO, the helper function obscures the test logic because the function name isn't telling me anything about what `stdlib_test_case` is doing. I have to jump to the definition to understand the test case. I would just inline the method or rename the method to `unwrap_standard_library(path: &str)`

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:794 on 2024-07-05 07:51_

We should avoid numbering tests. It makes it extremelly difficult to understand what a test is testing and, from it, derive what the expected behavior is. 

---

_@MichaReiser approved on 2024-07-05 07:57_

Nice! Let's try to get this merged today. I think the PR summary needs updating ;)

Most of my comments are nits. My only real concern are the tests in `path.rs`. You showed a lot of care for writing all of them but I don't think snapshot testing is the appropriate tool for that level of testing and I'm worried that the tests are aiming for exhaustiveness, testing all details of the implementation, rather than the relevant observed module resolution behavior. That's why I think we should try to reduce them to the most important tests that are also worth keeping when refactoring the code. As a measure of the cost of the tests: I just spent close to an hour trying to understand the tests and figuring out which one's are showing the most important behavior of the implementation. 

That's why I think we should reduce the tests and instead  aim to increase the module-resolver level testing (doesn't have to be part of this PR but I think that should be a goal for next week)


---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:296 on 2024-07-05 09:48_

yeah, I realised yesterday that I was overcomplicating it 😄 your suggestion of adding a `ModuleName::from_components()` method made it much easier to see how to do it more simply :)

---

_@AlexWaygood reviewed on 2024-07-05 09:48_

---

_@AlexWaygood reviewed on 2024-07-05 09:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/module_name.rs`:1 on 2024-07-05 09:49_

The `from_relative_path()` method is removed. A `from_components()` method is added instead.

---

_@AlexWaygood reviewed on 2024-07-05 09:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:356 on 2024-07-05 09:55_

> I agree, we should avoid resolving `TypeshedVersions` if they aren't needed. I think you can use an `Option` or `OnceCell` to cache the versions lazily. [doc.rust-lang.org/std/cell/struct.OnceCell.html](https://doc.rust-lang.org/std/cell/struct.OnceCell.html)

Ahh, that's very clever. Thanks!

> That's correct, but caching still has a non-trivial overhead of doing a lookup in a concurrent hash map and then checking if the revision is still fresh.

Hmm, fair enough. I guess I'm used to Python, where looking things up in a cache is ~free for a method decorated with `@functools.lru_cache` :-) But I guess in Rust, ~everything's faster than Python, so the cost of looking things up in a cache becomes significant.

---

_@AlexWaygood reviewed on 2024-07-05 12:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:356 on 2024-07-05 12:02_

It took me a while to figure out how to do this, as the lifetimes got quite complicated. In the end I succeeded, but I had to reintroduce an `Arc` in the `TypeshedVersions` struct rather than have the Salsa query return a reference.

---

_@AlexWaygood reviewed on 2024-07-05 12:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:468 on 2024-07-05 12:17_

Good point. I think you're right; validation here wouldn't be necessary.

---

_@AlexWaygood reviewed on 2024-07-05 12:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:468 on 2024-07-05 12:27_

Ah, no, validation _is_ necessary here. `absolute_path` isn't a `ModuleResolutionPath`, it's a `FileSystemPath`, so it hasn't been validated.

---

_@AlexWaygood reviewed on 2024-07-05 12:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:852 on 2024-07-05 12:54_

I was worried that this would make `push()` and `join()` quite expensive. But your reasoning makes sense. If it's an invariant important enough to be tested (which I think it is), it should be asserted in release builds as well as debug builds. I've made the change.

---

_@AlexWaygood reviewed on 2024-07-05 13:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/module_name.rs`:172 on 2024-07-05 13:01_

> But I think we can simplify the implementation

Does the `to_compact_string()` method allocate? The implementation I've written tries quite hard to avoid allocating if it's unnecessary

---

_@AlexWaygood reviewed on 2024-07-05 13:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:539 on 2024-07-05 13:02_

Thanks, I've done that. They were definitely necessary at some earlier point in this PR's history, but they no longer are. Annoying that Clippy didn't spot this :(

---

_@AlexWaygood reviewed on 2024-07-05 13:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:161 on 2024-07-05 13:15_

Done in https://github.com/astral-sh/ruff/pull/12141/commits/470f0c6f1595d9ee9eec65bbdbe31b6ebc1c494f

---

_@AlexWaygood reviewed on 2024-07-05 14:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:482 on 2024-07-05 14:12_

I think they're both needed. Only by testing both with py38 and py39 do we assert the important invariant that changing the target version results in different module-resolution behaviours even though the stubs are available on disk in both situations.

---

_@AlexWaygood reviewed on 2024-07-05 14:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:542 on 2024-07-05 14:18_

(I already copied over this test into `typeshed.rs` in a prior PR. I thought I'd deleted this commented out version, but I obviously hadn't.)

---

_@AlexWaygood reviewed on 2024-07-05 14:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:488 on 2024-07-05 14:22_

> It also depends on the real `VERSIONS` file

I don't think that's true. This test only uses the mock.

---

_@AlexWaygood reviewed on 2024-07-05 14:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/typeshed/versions.rs`:488 on 2024-07-05 14:23_

But I can definitely split this test up.

---

_@AlexWaygood reviewed on 2024-07-05 16:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:581 on 2024-07-05 16:16_

Thanks. I think I'm still figuring out exactly what's idiomatic and what's not when it comes to writing tests in Rust! I've removed snapshot testing for everything except the tests that are actually, and replaced it with simpler assertions.

---

_@AlexWaygood reviewed on 2024-07-05 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:583 on 2024-07-05 16:50_

Thanks! I've simplified the tests and reduced the overall number quite a lot.

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:38 on 2024-07-05 16:50_

I'm not sure I understand what restriction this is implementing. Why is it relevant only to the standard library, and only in the case where the component we are pushing has an extension?

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:28 on 2024-07-05 16:57_

Should we also be tracking whether the final component of the path already has an extension, and if so disallow pushing any further components? I guess it's not clear to me how we decide which validity invariants need to be enforced here, because it doesn't seem like we enforce all of them.

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:89 on 2024-07-05 16:57_

Note that here you check this condition in all cases, whereas above you check it only if there is an extension.

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 17:19_

I find it quite confusing in reading the code, and I think also it is error-prone, that we use the same type to represent both (a) a search path root, and (b) the path to an actual module. I think these are different things that need to obey different constraints (e.g. a search path root should never have any file extension at all) and provide different behaviors (e.g. a search path root shouldn't be mutable; we shouldn't ever push new components to it; many of the other methods provided here are also never used - and should never be used, because they aren't even meaningful - on a search root).

IMO it will be a lot clearer if we explicitly represent these as different types. Right now we only know which is which by context in the calling code; we get no help from the type system in differentiating two things that should never be confused with each other. I don't think it would even be that complex a change; it would just mean that in some cases where we currently "clone" a search root into an initial package path, we'd instead be explicitly constructing one type from the other.

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:333 on 2024-07-05 17:24_

I'm finding it quite hard to understand the handling of relative vs absolute paths. Here in this method we create instances of `ModuleResolutionPathRef` that contain relative paths, but then in the above methods it seems like we expect them to contain absolute paths, because for non-stdlib variants we just ignore the passed-in search path entirely.

I think we should establish some clarity/invariants around which kinds of paths should be absolute and which should be relative.

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:227 on 2024-07-05 17:25_

I really don't like that we ignore the passed-in search path entirely for non-stdlib variants in so many of these methods. It seems very error-prone. At the very least I'd want to see some debug assertions of consistency, though I'd rather have an API that didn't have redundant information in the first place (e.g. if module paths were enforced as always relative and required to be combined with a search path to be turned into an absolute path.)

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:182 on 2024-07-05 17:46_

Should this comment reference the appropriate section of the typing spec (which is what we actually care about) rather than the PEP?

Do we have any remaining intentional differences with the typing spec?

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:195 on 2024-07-05 17:48_

`ResolvedModuleResolutionSettings` is a kind of unfortunate name (due to the double use of "Resolved/Resolution" for very different meanings within the same name).

Could this just be `ModuleResolutionSettings` and the raw version be `RawModuleResolutionSettings` or `ModuleResolutionSettingsInput`?

---

_@carljm reviewed on 2024-07-05 17:54_

---

_@carljm reviewed on 2024-07-05 17:57_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 17:57_

It might even make sense that a package/module path is always represented as simply a relative path and an Arc to a search root. Then the "kind" is already encoded in the search root kind, and we can also eliminate all these cases of needing to make sure the passed-in search root kind matches the module path kind.

---

_@AlexWaygood reviewed on 2024-07-05 17:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:182 on 2024-07-05 17:59_

Yes, this comment is now out of date; good catch.

---

_@MichaReiser reviewed on 2024-07-05 19:08_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:195 on 2024-07-05 19:08_

I would leave it as is. Input is misleading because it gives the impression that it is a salsa input where it is not. We discussed alternatives, for example using a builder that directly constructs the "resolved" thing but decided to leave this for another day. I suspect that all of this will change anyway when we introduce settings 

---

_@MichaReiser reviewed on 2024-07-05 19:12_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 19:12_

Alex's first version used different types for each path variant and it added a ton of boilerplate code (about 1000 lines) that made it very hard to find the "important" bits. I also think that it didn't add much value because the only places where the different path types were used were in the match arms of that given variant. In which case using different types doesn't protect from much. If you screw up the variant name, than it is as easy to screw up the path type as well (and it's a very easy error to spot). 

I would think differently if the paths were exposed externally, which they are not. 

Whether we should expose the search path root from an actual path differently, I don't know. It's also unclear to me how the code would need to change for that. 

---

_@carljm reviewed on 2024-07-05 19:14_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 19:14_

I don't think "use different types for each path variant" is at all related to the suggestion I'm making here. I don't think that would be a good idea.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:206 on 2024-07-05 19:25_

`db`, should for consistency reasons always be the first argument

---

_Comment by @MichaReiser on 2024-07-05 19:40_

You can avoid the `Arc` by adding a few lifetimes:

```patch
Index: crates/red_knot_module_resolver/src/resolver.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_module_resolver/src/resolver.rs b/crates/red_knot_module_resolver/src/resolver.rs
--- a/crates/red_knot_module_resolver/src/resolver.rs	(revision 4c7a1061a9e11a1b404e1c5cf6c334a2890a73c1)
+++ b/crates/red_knot_module_resolver/src/resolver.rs	(date 1720208092508)
@@ -309,11 +309,11 @@
     None
 }
 
-fn resolve_package<'a, I>(
-    db: &dyn Db,
+fn resolve_package<'a, 'db, I>(
+    db: &'db dyn Db,
     module_search_path: &ModuleResolutionPathBuf,
     components: I,
-    typeshed_versions: &LazyTypeshedVersions,
+    typeshed_versions: &LazyTypeshedVersions<'db>,
     target_version: TargetVersion,
 ) -> Result<ResolvedPackage, PackageKind>
 where
Index: crates/red_knot_module_resolver/src/typeshed/versions.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_module_resolver/src/typeshed/versions.rs b/crates/red_knot_module_resolver/src/typeshed/versions.rs
--- a/crates/red_knot_module_resolver/src/typeshed/versions.rs	(revision 4c7a1061a9e11a1b404e1c5cf6c334a2890a73c1)
+++ b/crates/red_knot_module_resolver/src/typeshed/versions.rs	(date 1720207666907)
@@ -14,11 +14,13 @@
 
 use crate::db::Db;
 use crate::module_name::ModuleName;
+use crate::resolver::ResolvedModuleResolutionSettings;
 use crate::supported_py_version::TargetVersion;
+use crate::ModuleResolutionSettings;
 
-pub(crate) struct LazyTypeshedVersions(OnceCell<TypeshedVersions>);
+pub(crate) struct LazyTypeshedVersions<'db>(OnceCell<&'db TypeshedVersions>);
 
-impl LazyTypeshedVersions {
+impl<'db> LazyTypeshedVersions<'db> {
     #[must_use]
     pub(crate) fn new() -> Self {
         Self(OnceCell::new())
@@ -40,7 +42,7 @@
     pub(crate) fn query_module(
         &self,
         module: &ModuleName,
-        db: &dyn Db,
+        db: &'db dyn Db,
         stdlib_root: &FileSystemPath,
         target_version: TargetVersion,
     ) -> TypeshedVersionsQueryResult {
@@ -56,16 +58,14 @@
             // this should invalidate not just the specific module resolution we're currently attempting,
             // but all type inference that depends on any standard-library types.
             // Unwrapping here is not correct...
-            parse_typeshed_versions(db, versions_file)
-                .as_ref()
-                .unwrap()
-                .clone()
+            parse_typeshed_versions(db, versions_file).as_ref().unwrap()
         });
+
         versions.query_module(module, PyVersion::from(target_version))
     }
 }
 
-#[salsa::tracked]
+#[salsa::tracked(return_ref)]
 pub(crate) fn parse_typeshed_versions(
     db: &dyn Db,
     versions_file: VfsFile,
@@ -152,8 +152,8 @@
     }
 }
 
-#[derive(Debug, Clone, PartialEq, Eq)]
-pub(crate) struct TypeshedVersions(Arc<FxHashMap<ModuleName, PyVersionRange>>);
+#[derive(Debug, PartialEq, Eq)]
+pub(crate) struct TypeshedVersions(FxHashMap<ModuleName, PyVersionRange>);
 
 impl TypeshedVersions {
     #[must_use]
@@ -300,7 +300,7 @@
                 reason: TypeshedVersionsParseErrorKind::EmptyVersionsFile,
             })
         } else {
-            Ok(Self(Arc::new(map)))
+            Ok(Self(map))
         }
     }
 }
Index: crates/red_knot_module_resolver/src/path.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_module_resolver/src/path.rs b/crates/red_knot_module_resolver/src/path.rs
--- a/crates/red_knot_module_resolver/src/path.rs	(revision 4c7a1061a9e11a1b404e1c5cf6c334a2890a73c1)
+++ b/crates/red_knot_module_resolver/src/path.rs	(date 1720208063481)
@@ -108,11 +108,11 @@
     }
 
     #[must_use]
-    pub(crate) fn is_regular_package(
+    pub(crate) fn is_regular_package<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: &Self,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> bool {
         ModuleResolutionPathRef::from(self).is_regular_package(
@@ -124,11 +124,11 @@
     }
 
     #[must_use]
-    pub(crate) fn is_directory(
+    pub(crate) fn is_directory<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: &Self,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> bool {
         ModuleResolutionPathRef::from(self).is_directory(
@@ -158,11 +158,11 @@
     }
 
     /// Returns `None` if the path doesn't exist, isn't accessible, or if the path points to a directory.
-    pub(crate) fn to_vfs_file(
+    pub(crate) fn to_vfs_file<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: &Self,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> Option<VfsFile> {
         ModuleResolutionPathRef::from(self).to_vfs_file(
@@ -198,12 +198,12 @@
 
 impl<'a> ModuleResolutionPathRefInner<'a> {
     #[must_use]
-    fn query_stdlib_version(
+    fn query_stdlib_version<'db>(
         module_path: &'a FileSystemPath,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         stdlib_search_path: Self,
         stdlib_root: &FileSystemPath,
-        db: &dyn Db,
+        db: &'db dyn Db,
         target_version: TargetVersion,
     ) -> TypeshedVersionsQueryResult {
         let Some(module_name) = stdlib_search_path
@@ -216,11 +216,11 @@
     }
 
     #[must_use]
-    fn is_directory(
+    fn is_directory<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: Self,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> bool {
         match (self, search_path) {
@@ -241,11 +241,11 @@
     }
 
     #[must_use]
-    fn is_regular_package(
+    fn is_regular_package<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: Self,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> bool {
         fn is_non_stdlib_pkg(path: &FileSystemPath, db: &dyn Db) -> bool {
@@ -274,11 +274,11 @@
         }
     }
 
-    fn to_vfs_file(
+    fn to_vfs_file<'db>(
         self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: Self,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> Option<VfsFile> {
         match (self, search_path) {
@@ -386,11 +386,11 @@
 
 impl<'a> ModuleResolutionPathRef<'a> {
     #[must_use]
-    pub(crate) fn is_directory(
+    pub(crate) fn is_directory<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: impl Into<Self>,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> bool {
         self.0
@@ -398,11 +398,11 @@
     }
 
     #[must_use]
-    pub(crate) fn is_regular_package(
+    pub(crate) fn is_regular_package<'db>(
         &self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: impl Into<Self>,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> bool {
         self.0
@@ -410,11 +410,11 @@
     }
 
     #[must_use]
-    pub(crate) fn to_vfs_file(
+    pub(crate) fn to_vfs_file<'db>(
         self,
-        db: &dyn Db,
+        db: &'db dyn Db,
         search_path: impl Into<Self>,
-        typeshed_versions: &LazyTypeshedVersions,
+        typeshed_versions: &LazyTypeshedVersions<'db>,
         target_version: TargetVersion,
     ) -> Option<VfsFile> {
         self.0
@@ -794,7 +794,7 @@
         );
     }
 
-    fn py38_stdlib_test_case() -> (TestDb, ModuleResolutionPathBuf, LazyTypeshedVersions) {
+    fn py38_stdlib_test_case() -> (TestDb, ModuleResolutionPathBuf) {
         let TestCase {
             db,
             custom_typeshed,
@@ -802,12 +802,14 @@
         } = create_resolver_builder().unwrap().build();
         let stdlib_module_path =
             ModuleResolutionPathBuf::stdlib_from_typeshed_root(&custom_typeshed).unwrap();
-        (db, stdlib_module_path, LazyTypeshedVersions::new())
+        (db, stdlib_module_path)
     }
 
     #[test]
     fn mocked_typeshed_existing_regular_stdlib_pkg_py38() {
-        let (db, stdlib_path, versions) = py38_stdlib_test_case();
+        let (db, stdlib_path) = py38_stdlib_test_case();
+
+        let versions = LazyTypeshedVersions::new();
 
         let asyncio_regular_package = stdlib_path.join("asyncio");
         assert!(asyncio_regular_package.is_directory(
@@ -855,7 +857,9 @@
 
     #[test]
     fn mocked_typeshed_existing_namespace_stdlib_pkg_py38() {
-        let (db, stdlib_path, versions) = py38_stdlib_test_case();
+        let (db, stdlib_path) = py38_stdlib_test_case();
+
+        let versions = LazyTypeshedVersions::new();
 
         let xml_namespace_package = stdlib_path.join("xml");
         assert!(xml_namespace_package.is_directory(
@@ -886,7 +890,9 @@
 
     #[test]
     fn mocked_typeshed_single_file_stdlib_module_py38() {
-        let (db, stdlib_path, versions) = py38_stdlib_test_case();
+        let (db, stdlib_path) = py38_stdlib_test_case();
+
+        let versions = LazyTypeshedVersions::new();
 
         let functools_module = stdlib_path.join("functools.pyi");
         assert!(functools_module
@@ -903,9 +909,10 @@
 
     #[test]
     fn mocked_typeshed_nonexistent_regular_stdlib_pkg_py38() {
-        let (db, stdlib_path, versions) = py38_stdlib_test_case();
+        let (db, stdlib_path) = py38_stdlib_test_case();
 
         let collections_regular_package = stdlib_path.join("collections");
+        let versions = LazyTypeshedVersions::new();
         assert_eq!(
             collections_regular_package.to_vfs_file(
                 &db,
@@ -931,7 +938,8 @@
 
     #[test]
     fn mocked_typeshed_nonexistent_namespace_stdlib_pkg_py38() {
-        let (db, stdlib_path, versions) = py38_stdlib_test_case();
+        let (db, stdlib_path) = py38_stdlib_test_case();
+        let versions = LazyTypeshedVersions::new();
 
         let importlib_namespace_package = stdlib_path.join("importlib");
         assert_eq!(
@@ -972,7 +980,8 @@
 
     #[test]
     fn mocked_typeshed_nonexistent_single_file_module_py38() {
-        let (db, stdlib_path, versions) = py38_stdlib_test_case();
+        let (db, stdlib_path) = py38_stdlib_test_case();
+        let versions = LazyTypeshedVersions::new();
 
         let non_existent = stdlib_path.join("doesnt_even_exist");
         assert_eq!(
@@ -988,7 +997,7 @@
         ));
     }
 
-    fn py39_stdlib_test_case() -> (TestDb, ModuleResolutionPathBuf, LazyTypeshedVersions) {
+    fn py39_stdlib_test_case() -> (TestDb, ModuleResolutionPathBuf) {
         let TestCase {
             db,
             custom_typeshed,
@@ -999,12 +1008,13 @@
             .build();
         let stdlib_module_path =
             ModuleResolutionPathBuf::stdlib_from_typeshed_root(&custom_typeshed).unwrap();
-        (db, stdlib_module_path, LazyTypeshedVersions::new())
+        (db, stdlib_module_path)
     }
 
     #[test]
     fn mocked_typeshed_existing_regular_stdlib_pkgs_py39() {
-        let (db, stdlib_path, versions) = py39_stdlib_test_case();
+        let (db, stdlib_path) = py39_stdlib_test_case();
+        let versions = LazyTypeshedVersions::new();
 
         // Since we've set the target version to Py39,
         // `collections` should now exist as a directory, according to VERSIONS...
@@ -1057,7 +1067,9 @@
 
     #[test]
     fn mocked_typeshed_existing_namespace_stdlib_pkg_py39() {
-        let (db, stdlib_path, versions) = py39_stdlib_test_case();
+        let (db, stdlib_path) = py39_stdlib_test_case();
+
+        let versions = LazyTypeshedVersions::new();
 
         // The `importlib` directory now also exists...
         let importlib_namespace_package = stdlib_path.join("importlib");
@@ -1100,7 +1112,8 @@
 
     #[test]
     fn mocked_typeshed_nonexistent_namespace_stdlib_pkg_py39() {
-        let (db, stdlib_path, versions) = py39_stdlib_test_case();
+        let (db, stdlib_path) = py39_stdlib_test_case();
+        let versions = LazyTypeshedVersions::new();
 
         // The `xml` package no longer exists on py39:
         let xml_namespace_package = stdlib_path.join("xml");
```

You also don't need a new dependency. You can use `std::cell::OnceCell`

But all the methods that take a `db` as argument feel a bit awkward because of the many arguments. It makes me wonder if we shouldn't create a stateful `ModuleResolver` struct that handles a single resolution

```
struct ModuleResolver<'db> {
	db: &'db Db,
	versions: Option<&'db TypeshedVersions>,
	target_version: PyVersion
}

impl ModuleResolver {
	fn resolve_package(&mut self, module_resolution_path, components) -> Result<ResolvedPackage, PackageKind> {
		...
	}

	fn is_directory(&mut self, module_resolution_path: &ModuleResolutionPath) -> bool {
		...
	}

	fn is_regular_package(&mut self, ...)
}
```

The methods can all take `&self` if `versions` uses a `OnceCell`, but I also don't mind passing a `mut`. Anyway, I don't think we have to do this now. I do like @carljm suggestion. Let's revisit this when also making that change.


---

_@MichaReiser reviewed on 2024-07-05 20:02_

---

_@carljm reviewed on 2024-07-05 20:02_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 20:02_

After offline discussion with @MichaReiser, our thinking is that this might be an improvement, but it might be better to record it in a TODO comment and land the PR so we don't have a big PR outstanding for so long.

I'm ok with this: this is all pretty much encapsulated in a few Salsa queries with simple API, so it's not like we will be writing a bunch of new code directly depending on these path abstractions.

---

_@AlexWaygood reviewed on 2024-07-05 21:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:38 on 2024-07-05 21:42_

Uhmm well if a file in a custom typeshed directory is something like `foo.bar.baz.pyi`, something pretty weird is going on. But I think you're right that this probably isn't something we really need to assert here. There are many potential ways in which a user-supplied custom typeshed directory could be weird, and we shouldn't bend over backwards to check for all of them. At some point, we need to trust what the user gives us.

---

_@AlexWaygood reviewed on 2024-07-05 22:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 22:37_

Yeah, I agree that what you propose sounds cleaner, but I think what I have now is okay, and I'd like to land this now. If I have time next week, I'm happy to come back to this

---

_@AlexWaygood reviewed on 2024-07-05 22:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:79 on 2024-07-05 22:40_

I've added a TODO

---

_Merged by @AlexWaygood on 2024-07-05 22:43_

---

_Closed by @AlexWaygood on 2024-07-05 22:43_

---

_Branch deleted on 2024-07-05 22:43_

---
