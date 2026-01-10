```yaml
number: 19883
title: "[ty] Add completion support for `import` and `from ... import` statements"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/import-module
created_at: 2025-08-12T18:42:00Z
updated_at: 2025-08-20T14:27:56Z
url: https://github.com/astral-sh/ruff/pull/19883
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add completion support for `import` and `from ... import` statements

---

_Pull request opened by @BurntSushi on 2025-08-12 18:42_

This required adding what is effectively a second implementation of
module resolution in order to facilitate asking for "list modules"
instead of "resolve this module." While this does create some
duplication of some pretty hairy logic, there is also a substantial bit
of reuse. For example, `SearchPath` and `ModulePath` encapsulate a fair
bit of logic that we are able to reuse here.

Most of the work in this PR was in writing the tests and occasionally
fixing problems that they uncovered.

One thing this PR does not handle is caching. There are known cache
invalidation problems here. That is, if one adds a module to a search
path, completions may become stale. Similarly for deletions. I intend
to address this in another PR.

(This PR is best reviewed commit by commit.)


---

_Review requested from @carljm by @BurntSushi on 2025-08-12 18:42_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-08-12 18:42_

---

_Review requested from @sharkdp by @BurntSushi on 2025-08-12 18:42_

---

_Review requested from @dcreager by @BurntSushi on 2025-08-12 18:42_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-08-12 18:42_

---

_Review request for @dcreager removed by @BurntSushi on 2025-08-12 18:42_

---

_Review request for @carljm removed by @BurntSushi on 2025-08-12 18:42_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-08-12 18:42_

---

_Label `server` added by @BurntSushi on 2025-08-12 18:42_

---

_Label `ty` added by @BurntSushi on 2025-08-12 18:42_

---

_Comment by @BurntSushi on 2025-08-12 18:42_

Demo:

https://github.com/user-attachments/assets/5cc6b303-f09e-4cc8-a59a-62558115f1be



---

_Comment by @github-actions[bot] on 2025-08-12 18:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-12 18:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:68 on 2025-08-13 08:01_

I feel like something's missing in this sentence

```suggestion
        /// for other things, like only returning completions
        /// that start with a prefix corresponding to this token.
```



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:16 on 2025-08-13 08:20_

I know that you intentionally "ignored" caching as part of this PR. However, I do think that caching at the `db` level is undesired and it may change your design (e.g. a global `Lister` isn't possible). 


The issue with caching `list_modules` is that any file addition or removal invalidates the entire cache. I think a better approach is to instead cache at the search path level. Whether we need a cache for the aggregated result is less clear to me.

I think another approach here could be that we have one query for each search path and all it returns is a listing of all entries it contains. A second query than does the aggregation. I think that would somewhat nicely map to your current model, where the entries returned by the search-path-specific-query map to the arguments to `add_path` (or `add_module`?). In the future, we could decide to run those queries in parallel. This might also give us better incrementality because:

* We avoid traversing the directories unnecessarily and invalidation is "as easy as" reading the `revision` of the search path's file root
* The aggregation query only runs if any search path returns a different list of entries. 
* `resolve_file_module` already uses `File`-level APIs, it only invalidates if the specific files chane.


I don't think it's necessary to address this concern as this PR, unless it invalidates some of the design. I'll leave that up to you

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:80 on 2025-08-13 08:25_

Is the `Into` worth it here, given that there are very few call sites (and it's a fairly chunky method)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:50 on 2025-08-13 08:29_

Mainly curious because I think I can learn something here :). What's the motivation for using a `BTreeMap` here over a `FxHashMap`? Is it because `into_modules` returns a sorted list or are there other reasons?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:17 on 2025-08-13 08:35_

I wonder if we could have some form of integration test for `list_modules` that ensures that: for every module returned by list module, `resolve_module(module.name) == module`. Maybe that's something we could run at the end of each mdtest?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:476 on 2025-08-13 08:39_

You could extend `TestCaseBuilder` to allow customizing the underlying `System`. But I suspect that doesn't make it easier as you know rely on os behavior :)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:431 on 2025-08-13 08:41_

An alternative to the inline tests here could have been to extend `ty_extensions.internal` with a `list_modules` method so that you can then write mdtests for it. But I'm not sure if other folks would agree with this :)

It just feels a bit unfortunate that we now have some mdtests testing module resolution and a separate set of tests that verify the very same behavior for list modules

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/module.rs`:262 on 2025-08-13 08:44_

What do we need `Ord` for in `KnownModule`? I'm asking because some of those entries map to multiple actual modules and it doesn't really have a natural ordering.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:158 on 2025-08-13 08:57_

I think we now need to make `Module` a salsa interned or we risk that the same module resolved with `resolve_module` and its counterpart from `list_modules` won't compare equal because the identity of a tracked struct not only depends on the tracked struct's values but also the query that creates it. That means, any tracked struct by `list_modules` won't compare equal to any tracked struct created by `resolve_module` even if their values are identical. 

You may need to run ty on some larger projects to see if the need to go through a global lock (because interning...) has any negative performance impact (see https://github.com/astral-sh/ruff/pull/19867)



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:220 on 2025-08-13 09:05_

The comment here explains what the condition right beneath it does without saying why the condition is here in the first place. I think I'd change it to something like: Merging across search paths is only necessary for namespace packages. For all other modules, entries from earlier search paths take precedence.



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:268 on 2025-08-13 09:07_

Checking whether the file system is case sensitive can be fairly expensive (as expensive as writing a file to disk!). The best case is that it's already cached. Either way, I'm not sure if this is a worthwhile performance optimization or if we should just always call `eq_ignore_ascii_case` 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:413 on 2025-08-13 09:09_

Shouldn't it be enough to sort by module name? I don't think we should ever return multiple modules with the same name? 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:886 on 2025-08-13 09:13_

Nit: You can change the underlying system of a `TestDb` by using `db.use_system(other)`. See this test for an example https://github.com/astral-sh/ruff/blob/f34b65b7a00dfc61c5a54e4280b421c94151e677/crates/ty_python_semantic/src/module_resolver/resolver.rs#L2158-L2164

It's up to you if you think this simplifies anything or not.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:1657 on 2025-08-13 09:14_

That's a lot of tests :) Nice work

---

_@MichaReiser approved on 2025-08-13 09:16_

This is so cool. Thank you for working on this. 

I think we should add an experimental LSP setting and use it to gate the completion for now (off by default). We can remove it again once we fix caching. See https://github.com/astral-sh/ruff/commit/7b6abfb030653f81a1d7cdb2b92e78947d4e28c6 on how to do that

---

_@BurntSushi reviewed on 2025-08-13 11:40_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:80 on 2025-08-13 11:40_

Nice catch. I flipped the `.into()` to the caller.

---

_@BurntSushi reviewed on 2025-08-13 11:42_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:50 on 2025-08-13 11:42_

Yeah it's just for sorting. Nothing deeper. I just reached for the simplest thing.

---

_@BurntSushi reviewed on 2025-08-13 13:57_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:476 on 2025-08-13 13:57_

I think you mean threading down a parameter that would allow controlling the sort order of `MemoryFileSystem::read_directory` right? Yeah idk if it's worth it.

---

_@BurntSushi reviewed on 2025-08-13 14:03_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:431 on 2025-08-13 14:03_

Not just for listing modules, for `resolve_module` as well. That's where these tests came from. i.e., I copied the approach to testing `resolve_module`. The `TestCaseBuilder` was already there for me to help write tests. This approach also lets us do some things that might be tricky in the context of mdtest, like setting up symlinks. (Although admittedly most tests I think could be written in the style you suggest, assuming the existence of an internal `list_modules` method.)

---

_@BurntSushi reviewed on 2025-08-13 14:07_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/module.rs`:262 on 2025-08-13 14:07_

It just made it easy to sort `Vec<Module>` in order to give more sensible snapshots.

I can certainly use a different approach, but this was simplest and it didn't seem obviously wrong to me.

---

_@BurntSushi reviewed on 2025-08-13 14:08_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/module.rs`:262 on 2025-08-13 14:08_

I got rid of these impls and sorted on `known.as_str()` instead.

---

_@BurntSushi reviewed on 2025-08-13 14:13_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:158 on 2025-08-13 14:13_

I've made it an `interned` struct.

I'll try it on some larger repos.

---

_@BurntSushi reviewed on 2025-08-13 14:14_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:220 on 2025-08-13 14:14_

Yes, thank you, that is much clearer.

---

_@BurntSushi reviewed on 2025-08-13 14:17_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:268 on 2025-08-13 14:17_

Fine with me. I was thinking that if we always used `eq_ignore_ascii_case`, then we'd consider `foo.PY` as a module on a case sensitive file system. But maybe that's okay?

---

_@BurntSushi reviewed on 2025-08-13 14:17_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:268 on 2025-08-13 14:17_

There are also some uses of `is_case_sensitive` in the `resolve_module` implementation.

---

_@BurntSushi reviewed on 2025-08-13 14:20_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:413 on 2025-08-13 14:20_

Yup, you're right. When I wrote this part of the test harness, it came before my `Lister` abstraction and I was returning multiple modules of the same name. So sorting by more stuff was helpful. But with `Lister` almost completely ruling that out by construction, it's probably not worth the extra sort criteria.

This also let me revert my `Ord` additions.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:892 on 2025-08-13 14:22_

@MichaReiser RE <https://github.com/astral-sh/ruff/pull/19883/files#r2272642630>

That's what I'm doing here? (This test originated from `resolver.rs`.)

---

_@BurntSushi reviewed on 2025-08-13 14:22_

---

_@BurntSushi reviewed on 2025-08-13 14:22_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:886 on 2025-08-13 14:22_

Is this what you mean? https://github.com/astral-sh/ruff/pull/19883/files#r2273636330

---

_@BurntSushi reviewed on 2025-08-13 15:07_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:268 on 2025-08-13 15:07_

Turns out that the extension always has to be `py` or `pyi`, even on case insensitive file systems (I confirmed this on macOS at least), so this can get even simpler!

---

_@MichaReiser reviewed on 2025-08-13 15:09_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:892 on 2025-08-13 15:09_

Lol, right. Blind me. I think the only consideration here would be to add a `with_system` to the `TestBuilder` so that we could reuse some code around setting up the `ProgramSettings` but maybe that's not possible after all

---

_Comment by @BurntSushi on 2025-08-13 15:10_

I spoke with @MichaReiser offline and we agreed it'd probably be fine to land this without adding an experimental opt-in since I'm going to be working on caching next anyway. Plus, the bug related to caching can technically be worked around by restarting the LSP server (obviously not ideal). But, I'll wait to merge this until after the next release to give the biggest time window.

---

_@BurntSushi reviewed on 2025-08-13 15:11_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:892 on 2025-08-13 15:11_

Aye. I'll leave it as-is for now, since we only do this in a few places.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:158 on 2025-08-13 15:29_

I tried on our typical large repo test, and things seem okay:

```
$ time ty-main check > /dev/null
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ ?????/????? files                                                                                                                                                 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

real    21.397
user    2:34.59
sys     13.250
maxmem  20325 MB
faults  0

$ time ty-ag-import-module check > /dev/null
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ ?????/????? files                                                                                                                                                 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

real    21.490
user    2:35.97
sys     13.394
maxmem  20548 MB
faults  0
```

I tried it on several other projects and results were comparable.

---

_@BurntSushi reviewed on 2025-08-13 15:29_

---

_@BurntSushi reviewed on 2025-08-20 12:56_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:16 on 2025-08-20 12:56_

This is finally done in #19887.

---

_Merged by @BurntSushi on 2025-08-20 14:27_

---

_Closed by @BurntSushi on 2025-08-20 14:27_

---

_Branch deleted on 2025-08-20 14:27_

---
