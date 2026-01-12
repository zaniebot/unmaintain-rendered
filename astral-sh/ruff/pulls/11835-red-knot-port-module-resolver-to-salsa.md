```yaml
number: 11835
title: "red-knot: Port module resolver to salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-memory-resolver
created_at: 2024-06-11T12:41:14Z
updated_at: 2024-06-18T12:27:51Z
url: https://github.com/astral-sh/ruff/pull/11835
synced_at: 2026-01-12T15:55:39Z
```

# red-knot: Port module resolver to salsa

---

_@MichaReiser_

## Summary

This PR ports the module (*not* memory) resolver from red-knot to Salsa and moves it into `ruff_python_semantic`. Using Salsa requires us to use some `Vfs` methods instead of calling `path.exists`. In return, Salsa gives us automatic invalidation of the resolved modules when the file status in `Vfs` change (e.g. a file gets deleted or is added).

I had to add new functionality to `FileSystemPath` and `FileSystem` to implement the module resolver. 

## Open question

* Should we rename the Rust module `module` to `module_resolver`? Currently `module` contains the generic `Module` definition and `module/resolver` the resolver implementation. The reason why I'm inclined to move everything under `module_resolver` is that what's now defined as `Module` is only a `Module` in the sense of module resolution. It doesn't represent a module at runtime or the `ModuleType`.


## Test Plan

I ported all existing tests and added new tests that verify the invalidation logic.


---

_@MichaReiser reviewed on 2024-06-11 12:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module.rs`:84 on 2024-06-11 12:43_

I converted the examples to doctests and found bugs in both `parent` and `starts_with` :hand_over_mouth: 

---

_@MichaReiser reviewed on 2024-06-11 12:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module.rs`:212 on 2024-06-11 12:44_

What's different to the non-salsa version is that I removed `ModulePath` and instead made `search_path` and `file` (used to be called `path`) direct members of `Module`. It makes the API somewhat easier to use. 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module.rs`:303 on 2024-06-11 12:45_

This now stores a `VfsPath`

---

_@MichaReiser reviewed on 2024-06-11 12:45_

---

_@MichaReiser reviewed on 2024-06-11 12:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:138 on 2024-06-11 12:47_

I moved the field documentations to the fields and I renamed the structs. Otherwise unchanged.

---

_@MichaReiser reviewed on 2024-06-11 12:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:229 on 2024-06-11 12:48_

I expect that this will become a salsa-query long term where it internally queries the settings to resolve the typeshed paths. For now, a single function to prepare for this

---

_@MichaReiser reviewed on 2024-06-11 12:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:258 on 2024-06-11 12:50_

The `file.exists()` check now becomes a simple call that tries to resolve the `VfsFile` for the path. If that fails, then the file doesn't exist (or isn't accessible). The benefit of this is that this operation at the same time expresses that resolving `name` depends on whether `stub` exists and salsa will rerun the query if the `stub.status` changes.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:376 on 2024-06-11 12:51_

The tests are mostly 1:1 ports with the exception of the symlink test where we no longer tread symlinks as the same module as discussed on discord. 

There are a few new tests at the end that verify that the salsa invalidation works as expected.

---

_@MichaReiser reviewed on 2024-06-11 12:51_

---

_@MichaReiser reviewed on 2024-06-11 12:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:806 on 2024-06-11 12:52_

This test demonstrates how we can use the OS filesystem in a test for when we want to test more advanced FS behavior that's not implemented (and I have no intention to implement it) in MemoryFs.

---

_@MichaReiser reviewed on 2024-06-11 12:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:1 on 2024-06-11 12:53_

I had to add more methods to `FileSystemPath` because the module resolver (and `ModuleName`) perform path manipulations. The doctests are directly copied from `camino`. 

---

_@MichaReiser reviewed on 2024-06-11 12:54_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/memory.rs`:1 on 2024-06-11 12:54_

I wrote a few tests that demonstrate that Salsa retriggers module resolution when a file gets removed. This required adding support for removing files and directories.

Removing directories required changing the map from a regular hash map to a `BTreeMap` so that it becomes possible to search for all files in a directory.

---

_@MichaReiser reviewed on 2024-06-11 12:55_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs/path.rs`:1 on 2024-06-11 12:55_

I added a few more convenient implementations that make working with `VfsPath` more ergonomic (I got annoyed when writing tests)

---

_@MichaReiser reviewed on 2024-06-11 12:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/db.rs`:1 on 2024-06-11 12:56_

One mode resolver test, tests symlinking functionality that the memory file system doesn't support. The changes in here add support for swapping the memory file system with the real FS for tests.

---

_Label `red-knot` added by @MichaReiser on 2024-06-11 12:56_

---

_@MichaReiser reviewed on 2024-06-11 13:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/Cargo.toml`:14 on 2024-06-11 13:06_

Sorry @charliermarsh 

---

_Review requested from @carljm by @MichaReiser on 2024-06-11 14:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-11 14:28_

---

_Renamed from "red-knot: Port module resolver to salsa" to "red-knot[salsa part 6]: Port module resolver to salsa" by @MichaReiser on 2024-06-11 14:28_

---

_Comment by @MichaReiser on 2024-06-11 14:29_

Lol, what's this branch name... `memory_resolver`. I hope the code I wrote is better than my naming

---

_Marked ready for review by @MichaReiser on 2024-06-11 14:43_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-12 07:12_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-06-12 08:00_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:182 on 2024-06-12 14:03_

I need to add a to-do list item to check in what situations Python normalizes the import statement itself. E.g. `import foo.bar.baz` is obviously normalized to the same thing as `..bar.baz` and `.baz` in some situations, and I don't know how symlinks play into that. But I think it's correct that we shouldn't normalize the path here (the normalization of the import statement should probably happen at an earlier stage)

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:1 on 2024-06-12 14:14_

Overall, it feels like we have to expose a _lot_ of methods here that just delegate to the wrapped `Utf8Path` and `Utf8PathBuf` types, such that I wonder what we gain from the newtype wrappers. It might be simpler just to use `Utf8Path` and `Utf8PathBuf` directly? I might be missing some of the benefits here from the newtype wrappers.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:276 on 2024-06-12 14:19_

(The name of this wasn't immediately clear to me when I skimmed by it on the first read, until I looked at the surrounding code on the second read)

```suggestion
    /// Private method providing the implementation for `touch_path` and `touch`
    fn touch_impl(db: &mut dyn Db, path: &VfsPath, file: Option<VfsFile>) {
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/module.rs`:74 on 2024-06-12 14:23_

(Doesn't have to be dealt with here.) Another option might be to return a `Result` from this `new` method (similar to `regex::Regex::new()`). Then we could also add other assertions here that would also make sense, such as the fact that all components should be valid Python identifiers

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/module/resolver.rs`:73 on 2024-06-12 14:45_

I find this comment slightly confusing because you say that `path_to_module` resolves to a file, but the type signature says that it returns `Option<Module>`

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/module/resolver.rs`:185 on 2024-06-12 14:52_

haha, I see you got rid of my `chain`s 😆 I still honestly prefer them, but I won't insist

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/module/resolver.rs`:233 on 2024-06-12 14:55_

Because of the typeshed `VERSIONS` file, we will also need to add the target Python version as an input to this function at some point

---

_@AlexWaygood approved on 2024-06-12 14:56_

Looks great, thank you!

---

_@AlexWaygood reviewed on 2024-06-12 15:07_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:1 on 2024-06-12 15:07_

Ah I see you already discussed this with @carljm in https://github.com/astral-sh/ruff/pull/11802/files#r1635057752.

---

_@MichaReiser reviewed on 2024-06-12 15:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module.rs`:74 on 2024-06-12 15:16_

What I worry is that it makes the API fairly annoying to use. I need to check where we call `new` to see if returning `None` in these cases (or an error) would make the API cumbersome to use.

---

_@MichaReiser reviewed on 2024-06-12 15:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:73 on 2024-06-12 15:17_

What I mean by `resolves` is that it performs a vfs lookup to get the file for the given path. It's not about the return type. I can try to clarify this.

---

_@MichaReiser reviewed on 2024-06-12 15:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:233 on 2024-06-12 15:19_

Hmm, do we need to make it an argument to the function? Can't we assume the same python versions for all `resolve_module` calls? I don't think we want to support type checking programs where part of the program use python 3.8 and another part Python 3.9

---

_@AlexWaygood reviewed on 2024-06-12 15:21_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/module/resolver.rs`:233 on 2024-06-12 15:21_

Ah, yes, we can absolutely assume the same Python version for all `resolve_module` calls within any single invocation of Ruff. I suppose I was thinking of this function as a "pure" function where all the inputs were part of the function signature. But if that doesn't work with Salsa, so be it :-)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:233 on 2024-06-12 15:39_

It does work with Salsa, but it does require that we intern the python versions. That wouldn't be a big deal by itself, but seems cumbersome to use if we can assume the same python version anyway. 

---

_@MichaReiser reviewed on 2024-06-12 15:39_

---

_@AlexWaygood reviewed on 2024-06-12 15:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/module/resolver.rs`:233 on 2024-06-12 15:42_

I do feel like I like the clarity of having all necessary inputs for module resolution being part of the function signature, but I don't have a very strong opinion

---

_@MichaReiser reviewed on 2024-06-12 15:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:233 on 2024-06-12 15:52_

I think we can add it to the method that `resolve_module` calls internally. 

But I think it would be very awkward if all upstream queries have to call `resolve_module(db, "name", python_version(db))` just because we want it to be part of the API (on top of the issue that it now needs to be interned). 

---

_@MichaReiser reviewed on 2024-06-13 07:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module/resolver.rs`:185 on 2024-06-13 07:57_

I tried... but it broke my brain to find the compile errors with the very long expression. 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/module.rs`:74 on 2024-06-13 08:17_

I changed `new` to return an `Option` and added a new `new_static` method for when working with static module names ("avoids allocations all together)

---

_@MichaReiser reviewed on 2024-06-13 08:17_

---

_Comment by @github-actions[bot] on 2024-06-13 09:00_

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

_Renamed from "red-knot[salsa part 6]: Port module resolver to salsa" to "red-knot: Port module resolver to salsa" by @MichaReiser on 2024-06-18 12:08_

---

_Merged by @MichaReiser on 2024-06-18 12:11_

---

_Closed by @MichaReiser on 2024-06-18 12:11_

---

_Branch deleted on 2024-06-18 12:12_

---
