```yaml
number: 10849
title: Kick off Red-knot
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot
created_at: 2024-04-09T16:27:18Z
updated_at: 2024-04-27T08:43:16Z
url: https://github.com/astral-sh/ruff/pull/10849
synced_at: 2026-01-10T22:37:01Z
```

# Kick off Red-knot

---

_Pull request opened by @MichaReiser on 2024-04-09 16:27_

The beginning of multifile analysis. We'll eventually merge this with ruff but are using a dedicated crate to flesh out the basic infrastructure first.

---

_Comment by @codspeed-hq[bot] on 2024-04-09 16:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot)

### Merging #10849 will **not alter performance**

<sub>Comparing <code>red-knot</code> (dd4748b) with <code>main</code> (845ba7c)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @carljm on `crates/red_knot/src/check.rs`:11 on 2024-04-10 07:18_

It looks like it is possible to have lazy inputs in salsa: https://salsa-rs.github.io/salsa/common_patterns/on_demand_inputs.html

---

_Review comment by @carljm on `crates/red_knot/src/check.rs`:14 on 2024-04-10 07:28_

This could be an issue. In looking at the six year old salsa issue for persistent cache, I found this comment from someone with experience building these kinds of tools that recommends against persistent caches entirely: https://github.com/rust-lang/rfcs/pull/1317#issuecomment-150965895

I’m not convinced, though, particularly for the CLI use case where laziness doesn’t help much. 

---

_@carljm reviewed on 2024-04-10 07:28_

---

_@MichaReiser reviewed on 2024-04-10 08:57_

---

_Review comment by @MichaReiser on `crates/red_knot/src/check.rs`:11 on 2024-04-10 08:57_

Yeah I saw that. Trying it right now. There's still the problem that we can never purge the files 

---

_@MichaReiser reviewed on 2024-04-10 08:59_

---

_Review comment by @MichaReiser on `crates/red_knot/src/check.rs`:14 on 2024-04-10 08:59_

Yeah, I think this is different for a CLI where you e.g. want to check your entire program. I think rust-analyzer does some indexing during startup and that can easily take 10 seconds. I don't think our users would appreciate it if Ruff suddenly takes 10s in CI.

---

_@carljm reviewed on 2024-04-10 23:56_

---

_Review comment by @carljm on `crates/red_knot/src/files.rs`:63 on 2024-04-10 23:56_

So I found this a bit confusing, let me see if I understand it now. This looks like effectively a hashset of `FileId`, but the reason the name `by_path` makes sense for it is that we actually use the hash of the path as the hash of each `FileId` in the set, as a way to determine if a path is already in the set, without actually storing the path in the set. Is that about right?

---

_@carljm reviewed on 2024-04-10 23:59_

---

_Review comment by @carljm on `crates/red_knot/src/files.rs`:78 on 2024-04-10 23:59_

why do we need to re-hash here when we definitely already hashed the same path above?

---

_Review comment by @carljm on `crates/red_knot/src/files.rs`:78 on 2024-04-11 02:17_

Oh, I guess this isn't immediately  rehashing, it's providing a hash function to be used later.

---

_@carljm reviewed on 2024-04-11 02:17_

---

_@MichaReiser reviewed on 2024-04-11 07:21_

---

_Review comment by @MichaReiser on `crates/red_knot/src/files.rs`:63 on 2024-04-11 07:21_

Yes, the idea is to avoid allocating each path twice: once in the `Map` and once in the index. 

It indeed is a `HashSet` but the `hashbrown::HashSet` doesn't provide access to `raw_entry` (at least the last time I checked). But it's not really a big deal because `HashSet<V> = HashMap<V, ()>` anyway.

---

_@MichaReiser reviewed on 2024-04-11 07:22_

---

_Review comment by @MichaReiser on `crates/red_knot/src/files.rs`:78 on 2024-04-11 07:22_

Yeah, it's a hash function to rehash when growing the hash map. But it's possible that we could use the pre-computed hash.

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:146 on 2024-04-11 12:25_

Whoopsi, this should be create_id

---

_@MichaReiser reviewed on 2024-04-11 12:25_

---

_@carljm reviewed on 2024-04-11 13:26_

---

_Review comment by @carljm on `crates/red_knot/src/files.rs`:63 on 2024-04-11 13:26_

Right; using a hashmap with unit values as a hashset wasn't confusing to me; the confusing part is that from the type annotation it looks like just a hashset of FileId but "really" it's a hashset of paths, with the file id as just kind of a placeholder. 

---

_Review comment by @carljm on `crates/red_knot/Cargo.toml`:26 on 2024-04-12 00:06_

Why depend on `withered-magic` fork of salsa rather than on the main salsa repo (at `salsa-rs/salsa`)?

---

_Review comment by @carljm on `crates/red_knot/src/ast_ids.rs`:30 on 2024-04-12 00:33_

Why do we need this marker?

---

_Review comment by @carljm on `crates/red_knot/src/ast_ids.rs`:203 on 2024-04-12 04:35_

I'm not sure I understand the function of this `AstNodeKey` wrapper around `SyntaxNodeKey`, or why we need it. I'm guessing it has to do with typing and the PhantomData marker, but I haven't put it all together.


---

_Review comment by @carljm on `crates/red_knot/src/hir.rs`:4 on 2024-04-12 04:56_

I don't see where an arena is used; is this an aspirational comment, or a comment borrowed from rust-analyzer describing something we aren't doing here (yet)?

---

_Review comment by @carljm on `crates/red_knot/src/ast_ids.rs`:20 on 2024-04-12 05:03_

Why do we need this wrapper around an `AstId`?

---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:297 on 2024-04-12 05:11_

Any reason this is defined in `db.rs` rather than either in `ast_id.rs` or `hir.rs`?

---

_@carljm reviewed on 2024-04-12 05:28_

---

_@carljm reviewed on 2024-04-12 05:41_

---

_Review comment by @carljm on `crates/red_knot/src/ast_ids.rs`:96 on 2024-04-12 05:41_

I think that functions should be deferred but class bodies probably should not be, because at runtime the class body is evaluated immediately to create the class, and anything referenced in it needs to already be defined. and we need the class body to know the members of the class

---

_@MichaReiser reviewed on 2024-04-12 07:38_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:26 on 2024-04-12 07:38_

Lol, because I did copy paste this... 

---

_@MichaReiser reviewed on 2024-04-12 07:41_

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:30 on 2024-04-12 07:41_

Rust requires for structs that each type parameter is used by at least one field. Now, `AstId` isn't generic over `N`. 

We can work around this by using `PhantomData`. `PhantomData` has no runtime cost (it compiles down to a zero size type) and it's only purpose is to capture the type `N`. 

You can think of the pattern applied here as compile-time only generics similar to Java where the generic arguments are erased at runtime but we want them at compile time to catch typing errors (e.g. we want to prevent that you use a `AstId` for an `IfStmt` to load a `FunctionDef`).

---

_@MichaReiser reviewed on 2024-04-12 07:42_

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:203 on 2024-04-12 07:42_

Yes. You can think of it as compile-time generics:

* `SyntaxNodeKey`: The "runtime" value with erased type generic type arguments
* `AstNodeKey`: The "compile-time" generic type that prevents using an `AstNodeKey<IfStmt>` to retrieve an `AstNodeKey<FunctionDef>`. 

---

_@MichaReiser reviewed on 2024-04-12 07:44_

---

_Review comment by @MichaReiser on `crates/red_knot/src/hir.rs`:4 on 2024-04-12 07:44_

The `Module` has an `IndexVec` for each node type which "acts" as an arena because we don't make a heap allocation for each node but instead make a single vector allocation and write each node into that vec.

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:20 on 2024-04-12 07:44_

This is related to the PhantomData. See https://github.com/astral-sh/ruff/pull/10849#discussion_r1562158940

---

_@MichaReiser reviewed on 2024-04-12 07:44_

---

_@MichaReiser reviewed on 2024-04-12 07:44_

---

_Review comment by @MichaReiser on `crates/red_knot/src/db.rs`:297 on 2024-04-12 07:44_

No, there's none. I should probably have moved it when I introduced `hir.rs`

---

_@MichaReiser reviewed on 2024-04-12 07:53_

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:96 on 2024-04-12 07:53_

Maybe, I don't know. I think the ID generation is tightly tied to the HIR design and it's currently unclear to me what the HIR should capture exactly (as discussed over Discord). 

Overall, the goal of the HIR is to isolated changes and I could see it being beneficial if adding a class member doesn't change the IDs of functions coming after. The assignment of the ID also doesn't mean that these nodes don't have IDs. It only means that they have less stable IDs than the class itself. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:119 on 2024-04-18 08:19_

Nit: We should consider using `smol_str` to avoid allocating for short symbol names. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:85 on 2024-04-18 08:20_

Nit: I would remove `Default` because a `SymbolTable` without a module scope is invalid. 

---

_@MichaReiser reviewed on 2024-04-18 08:22_

---

_@carljm reviewed on 2024-04-18 18:42_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:85 on 2024-04-18 18:42_

Done.

---

_@carljm reviewed on 2024-04-18 18:45_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:119 on 2024-04-18 18:45_

Done.

---

_@carljm reviewed on 2024-04-18 18:50_

---

_Review comment by @carljm on `crates/red_knot/src/ast_ids.rs`:30 on 2024-04-18 18:50_

The thing I find a bit odd here is the `fn() -> N`; why is it not just `PhantomData<N>`? It seems like putting the typevar in return position like that might be intended to make `TypedAstId` contravariant in `N` rather than variant? But since Rust doesn't have struct subtyping, I'm not really sure what that would even mean; I thought Rust only really has variance for lifetimes.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:40 on 2024-04-18 21:13_

random musing: single string is more storage-efficient and doesn't require heap allocation for short module names, but it means we'll have to spend time splitting it by dot. Will it be more efficient overall to store it as a vector and avoid repeated splitting?

Maybe we don't actually split it by dot more than once, if we are caching the resolution to `ModulePath` anyway.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:54 on 2024-04-18 21:23_

Perhaps somewhere in here we should validate that the extensions is `.py` or `.pyi`?

This actually makes me think that perhaps `from_relative_path` should not exist at the `ModuleName` layer but at a layer where it returns several pieces of information: a `ModuleName`, a `ModuleKind` (e.g. `::Python` or `::Stub`), and also maybe `is_package` boolean (e.g. true for `foo/__init__.py`, false for `foo.py`). We will need all of those at some point; as implemented currently this `from_relative_path` is a lossy operation, since it returns only one of those.

Perhaps those other two fields, and this method, belong on `Module/ModuleData`?

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:49 on 2024-04-18 21:28_

The algorithm here will be simpler if instead of taking a full `Path` for `_to` parameter, we just take a `ModuleName` and `is_package` boolean (or some structure that encompasses both, might just be `Module`).

We need `is_package` because in `foo/bar/__init__.py`, `from . import baz` means `foo.bar.baz`, but from `foo/bar.py`, `from . import baz` means `foo.baz`. 

If we take a full `Path` here, then we effectively have to re-implement (or call) `from_relative_path` again as part of this method, but in the places we will likely call this from (resolving a relative import we find in the AST) we will already have all the data of the current module handy, so it will be wasteful to recalculate that from `Path`.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:68 on 2024-04-18 21:32_

I think there's a missing "push dot if name not empty" here? (Unless the above "push dot" case is moved down as I suggested.)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:42 on 2024-04-18 21:33_

This could be moved below the `name.push_str(...)` immediately following, and then it wouldn't need the "if name is empty" check, and it would also eliminate the need for another "push dot if name not empty where I commented below.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:70 on 2024-04-18 21:36_

We need a check here for the special case where `path.file_stem() == "__init__"`, in which case we should not push that to the module name; in fact we should remove a trailing dot instead and be done.

(In Python I'd avoid the need to manually add/remove dots by building up a list of strings instead of a single string, and then joining them on dot once done. But that may not be quite as efficient.)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:180 on 2024-04-18 21:56_

So this means we duplicate every module name, once inside the `ModuleData` and once inside this hashmap. In the symbol table I used your hashing trick from `Files` to avoid this double storage. Do you think that's generally worth it, or no?

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:423 on 2024-04-18 22:12_

Why should these be separated? What's the use case for resolving a module and not getting its SourceText in the same operation? I think in the doc I suggested that the resolver would query the VFS for a path and get back both the resolved path and the sourcetext at the same time (or "does not exist") which would eliminate this issue.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:315 on 2024-04-18 23:23_

I think this function shouldn't need to know anything about the specifics of the priority algorithm; that should be the responsibility only of `resolve_name`. In principle all we need to do here is compare the fully normalized version of the path we were given with the fully normalized path of the resolved module, and if they are not the same, then clearly the module name doesn't resolve to this file, and we return `None`.

Comparing roots first could be fine if its an optimization, but it shouldn't be necessary for correctness.

The inner "starts_with" check doesn't really make sense to me, it seems overfit to one particular priority case (`__init__.py`), and not necessary; it could just be an equality check instead. (Maybe this is covering for the bug in `from_relative_path` where it doesn't handle `__init__.py` correctly?)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:322 on 2024-04-18 23:50_

I think the other approach here is that this method should take a path, not a ModuleId (more parallel to `add_module`). Because as I understand it, the only use case for this is the VFS telling us that a file is deleted. In that case the VFS will already have the path, not a ModuleId. It seems like resolving the path to a ModuleId should be our responsibility.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:356 on 2024-04-19 00:08_

It is not actually required for a Python package to contain `__init__.py` or `__init__.pyi`, so we don't need to check that here.

There's actually a more subtle thing here we might need to support for correct resolution, which I failed to mention in the doc.

If a directory does not contain `__init__.py`, then it is a "namespace package" which can be spread across multiple search paths. In other words, `foo.bar` could resolve to `search-path-one/foo/bar.py` while `foo.baz` could resolve to `search-path-two/foo/baz.py`. But if `search-path-one/foo/__init__.py` exists (and `search-path-one` is higher priority than `search-path-two`), then that is not allowed! No module under `foo.` can resolve to anywhere other than `search-path-one/foo/...`.

You can see https://peps.python.org/pep-0420/ for the gory details, but I think I summarized it pretty well above :)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:374 on 2024-04-19 00:27_

I think ultimately we will want to separate responsibility for "generating list of candidate paths" (`ModuleResolver` must do this) from "checking what path actually exists" (VFS should do this, because it might have unsaved contents from editor too). (And ideally I think "check if file exists" and "read file contents" should be a single atomic operation to avoid race conditions; or at least they should always happen in the same method, one right after the other, so we can handle a read error the same as "doesn't exist" )

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:372 on 2024-04-19 00:29_

@AlexWaygood also pointed out that there are a few extra wrinkles we'll want to implement support for at some point, outlined in https://peps.python.org/pep-0561/#type-checker-module-resolution-order

Specifically, the extra wrinkles are:
1. We should give users a way to add extra search paths at the start of the list (this just generally makes sense as a feature.)
2. When looking at third-party packages in `site-packages`, we should a) look for a `foo-stubs/` package first if looking for types for the `foo/` package, and b) ignore types from a package that doesn't include a `py.typed` file.

I don't think this is high priority to implement now, but we should probably at least have a TODO for it.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:626 on 2024-04-19 00:52_

As mentioned above, this test isn't quite right, because namespace packages (packages without `__init__.py`) are also allowed.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:678 on 2024-04-19 01:19_

```suggestion
        std::os::unix::fs::symlink(&foo, bar.clone())?;
```

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:904 on 2024-04-19 01:23_

Yes, we definitely need to consider it as separate modules, since that's how Python will see it.

But you're right that we can actually store SourceText, physical-line diagnostics, and SyntaxTree keyed only by FileId, not by ModuleId -- that's a great point I hadn't considered.

If it adds any complexity it may not be worth it, though, because I think this case is very rare in practice: there's no good use case for having two identical modules in your source tree. (In fact it's generally a really bad idea, since it will lead to very confusing results like "expected FooClass, but got FooClass" etc.)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:905 on 2024-04-19 01:24_

Would it be worth asserting here also that the modules should actually share the same `FileId`?

---

_@carljm reviewed on 2024-04-19 01:24_

The module stuff looks great! Sorry for leaving so many comments; many of them can probably just turn into TODOs for now.

---

_Renamed from "Exploration of a salsa like compilation model (does not compile)" to "Red Knot" by @MichaReiser on 2024-04-19 05:58_

---

_@MichaReiser reviewed on 2024-04-19 06:01_

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:30 on 2024-04-19 06:01_

The reason is (and I was surprised by it) that `Phantom<Data>` made the type non-copyable, `NonEq` etc. 

I guess the alternative would have been to implement all these types manually https://users.rust-lang.org/t/how-to-copy-phantomdata-of-un-clone-able-types/82229

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:40 on 2024-04-19 06:04_

I thought about this as well and don't have a final conclusion. The good thing, it's nicely abstracted away, so we can play with it when we have real world usage. 

My thinking why I left it as is:

* We could use a `SmallVec` with a size of 2-3 in combination with a `SmolStr`. That means we should avoid allocating in many cases. However, it does have the downside that `ModuleName` now becomes a 3 * 24 bytes struct. That's probably not too bad because it is not used in many places but something to consider. 
* The worst case is that we need 1 + n where `n` is the number of segments allocations
* I think splitting by `.` is cheap. `PathBuf` uses a single vector internally and `.components` splits on demand. 

I went with the string because it felt simpler for now and I don't think splitting by components is a very common (hot) operation.

---

_@MichaReiser reviewed on 2024-04-19 06:04_

---

_@MichaReiser reviewed on 2024-04-19 06:08_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:54 on 2024-04-19 06:08_

I think `ModuleKind` and whether it is a package belongs on `ModuleData`. `ModuleName` is just the full qualified name of a package. The only reason I see for adding them to `ModuleName` is if we want to support explicitly querying modules by their kind, but I don't think this is something we want. That's why I think `from_relative_path` is fine. All it should do is to create the full qualified name from a relative import (it shouldn't perform any IO).

> Perhaps somewhere in here we should validate that the extensions is .py or .pyi?

Yeah, early returning when it is not a `py` or `pyi` file makes sense.





---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:49 on 2024-04-19 06:10_

That makes sense, it also avoids the need to check if the file has a `.py` extension haha

The only challenge with this is that we need to analyze files that aren't modules (and may not have a module name):

* Jupyter notebooks
* Files that don't have a `py` or `pyi` extension (Ruff supports configuring additional extensions that should be handled as python files)

---

_@MichaReiser reviewed on 2024-04-19 06:10_

---

_@MichaReiser reviewed on 2024-04-19 06:13_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:180 on 2024-04-19 06:13_

It would be nice if we could do that but I didn't find a way using `DashMap` because it doesn't expose `RawEntry`. 

An alternative that I considered is to wrap `ModuleName` in an `Arc`, but that kind of defeats the purpose of using a `SmalStr` (In this case we can just use a `Arc<str>`). 

My current thinking is: Resolving modules is expensive because of all the IO and the additional allocation shouldn't matter (there are also not that many modules). 

---

_@MichaReiser reviewed on 2024-04-19 06:18_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:423 on 2024-04-19 06:18_

It would result in us eagerly loading files even if they're unchanged. 

Let's say we load a persistent cache and only have to recheck a single file. The deserialization layer of the persistent cache must acquire module ids for each cached file (and potentially remap them in case it gets assigned a new module id). What we want to avoid is to load the file content at this stage (because it would be very expensive). 

Overall, I think there are a handful operations in the analysis where we just need to know to which module an import resolves but we don't want to analyze the file's content yet (and may not have to because it's either a local analysis or we determine that we have all data cached)

---

_@MichaReiser reviewed on 2024-04-19 06:20_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:322 on 2024-04-19 06:20_

I don't disagree but I just don't know. I think that's something we can change when using the API. 

---

_@MichaReiser reviewed on 2024-04-19 06:22_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:374 on 2024-04-19 06:22_

I'm not sure I understand. What other use cases do we have for getting the list of candidate paths? 

I do agree that we should use the VFS here. But I intentionally didn't build this out yet and used the real file system as a pretty good mock ;)

>  (And ideally I think "check if file exists" and "read file contents" should be a single atomic operation to avoid race conditions; or at least they should always happen in the same method, one right after the other, so we can handle a read error the same as "doesn't exist" )

See my comment above. I think we just need to handle this gracefully upstream. The `ModuleResolver` must query the (V)FS here to resolve the path, but we don't necessarily want to load that file's content yet. For example, we need to know the module to test if we can resolve a cached type.

---

_@MichaReiser reviewed on 2024-04-19 06:23_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:904 on 2024-04-19 06:23_

`ModulePath` gives you a `FileId` which uniquely identifies the file. So that TODO is actually resolved.

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:315 on 2024-04-19 06:27_

> I think this function shouldn't need to know anything about the specifics of the priority algorithm; that should be the responsibility only of resolve_name. In principle all we need to do here is compare the fully normalized version of the path we were given with the fully normalized path of the resolved module, and if they are not the same, then clearly the module name doesn't resolve to this file, and we return None.

I'm okay testing by normalized path. I used startswith because of my comment in `resolve_package`

```
assert_eq!(foo_id, resolver.resolve_path(&foo_path));

  // TODO: Should resolving by the directory name resolve a module or not?
  assert_eq!(foo_id, resolver.resolve_path(&foo_dir));

  Ok(())
```

where `foo_dir` is `foo` but there's only a `foo/__init__.py`. So the question is if that should work. If yes, then we need a check similar to what I have, otherwise we don't :P


---

_@MichaReiser reviewed on 2024-04-19 06:27_

---

_@MichaReiser reviewed on 2024-04-19 06:36_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:70 on 2024-04-19 06:36_

Nice find. I now pop `__init__` at the entry of the function. 

> (In Python I'd avoid the need to manually add/remove dots by building up a list of strings instead of a single string, and then joining them on dot once done. But that may not be quite as efficient.)

Yeah, we could do this here too. The way I would implement this (to avoid an extra `Vec` allocation) is to join two iterators, one over `components` and one with the module name. The only issue is that `to_str` can return `None` in case the path isn't UTF8, in which case this function should return `None`. That's where a single `iter.chain(other_iter).map(...).collect()` doesn't work anymore.

---

_@MichaReiser reviewed on 2024-04-19 06:41_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:356 on 2024-04-19 06:41_

Okay, so that means my current implementation implements namespace packages... but not "normal" packages. 

I'm not sure how much of this I want to implement now, a 100% correct module resolver isn't really a goal of the prototype. 

One thing we have to consider too is the case where` foo` contains no `__init__.py` but `foo/baz` does. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:372 on 2024-04-19 06:44_

> We should give users a way to add extra search paths at the start of the list (this just generally makes sense as a feature.)

In general? This should be "easy", it just means initializing `search_paths` with more entries



---

_@MichaReiser reviewed on 2024-04-19 06:44_

---

_@MichaReiser reviewed on 2024-04-19 06:45_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:678 on 2024-04-19 06:45_

Thanks. No need to clone, we can pass a reference `&bar`

---

_@MichaReiser reviewed on 2024-04-19 06:48_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:581 on 2024-04-19 06:48_

Hmm this is kind of annoying. It now tests more than it should. Anyway, I think we should add a helper for the assertion to avoid that future us add a test that incorrectly compares paths in the future

---

_@MichaReiser reviewed on 2024-04-19 06:51_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:581 on 2024-04-19 06:51_

Actually. I think the better idea is to fixup the `TestCase` construction and canonicalize the directory paths there (once)

---

_@MichaReiser reviewed on 2024-04-19 09:00_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:626 on 2024-04-19 09:00_

I added more tests. I hope my understanding of namespace packages is correct. But I think it should definitely be sufficient for this prototype ;)

---

_@MichaReiser reviewed on 2024-04-19 10:57_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:423 on 2024-04-19 10:57_

Another reason for storing them separately is that we probably want to implement some garbage collection mechanism for source text and AST but I don't think it is necessary for modules, because the data that we store is relatively small. 

The idea for source text and the AST is that we can evict files that haven't been from the cache. For example, we may need to load all files for the initial checking but we can evict most data from the cache (and only keeping the type level cache) once the initialization is over.

---

_@MichaReiser reviewed on 2024-04-19 12:47_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:144 on 2024-04-19 12:47_

Ideally, Iterators don't allocate. 

We can rewrite the iterator to be generic over any `Iterator` that returns a `SymbolId`.


```rust
struct SymbolIterator<'a, I> {
    table: &'a SymbolTable,
    ids: I,
}

impl<'a, I> Iterator for SymbolIterator<'a, I>
where
    I: Iterator<Item = SymbolId>,
{
    type Item = &'a Symbol;

    fn next(&mut self) -> Option<Self::Item> {
        let id = self.ids.next()?;
        Some(&self.table.symbols_by_id[id])
    }

    fn size_hint(&self) -> (usize, Option<usize>) {
        self.ids.size_hint()
    }
}

impl<'a, I> FusedIterator for SymbolIterator<'a, I> where
    I: Iterator<Item = SymbolId> + FusedIterator
{
}

impl<'a, I> ExactSizeIterator for SymbolIterator<'a, I> where
    I: Iterator<Item = SymbolId> + ExactSizeIterator
{
}

impl<'a, I> DoubleEndedIterator for SymbolIterator<'a, I>
where
    I: DoubleEndedIterator + Iterator<Item = SymbolId>,
{
    fn next_back(&mut self) -> Option<Self::Item> {
        let id = self.ids.next_back()?;
        Some(&self.table.symbols_by_id[id])
    }
}
```

You can then use it with

```rust
    pub(crate) fn symbols_for_scope(
        &self,
        scope_id: ScopeId,
    ) -> SymbolIterator<std::iter::Copied<hashbrown::hash_map::Keys<SymbolId, ()>>> {
        let scope = &self.scopes_by_id[scope_id];
        SymbolIterator {
            table: self,
            ids: scope.symbols_by_name.keys().copied(),
        }
    }
```

It could make sense to define an alias for it. Note. the old implementation poped the IDs from the back (not sure why). The new implementation consumes the Ids in the order returned by `keys`. 

General advice when implementing iterators:

* Override `size_hint` if you can estimate the number of elements. It is used when e.g. collecting into a vec
* Implement `FusedIterator` when it is guaranteed that the iterator returns no `Some` elements after it returned one `None` element. 
* Implement `DoubleEndedIterator` and `ExactSizeIterator` if possible.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:49 on 2024-04-19 14:52_

I don't know much about Jupyter notebooks, but I would assume they just can't have relative imports?

For files that don't have a `py` or `pyi` extension, I would assume we still treat them as modules as if they did? I'm not sure what the use cases for this is.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:54 on 2024-04-19 14:54_

Oh, I agree this data belongs on `ModuleData`, not on `ModuleName`, I wasn't suggesting to put it on `ModuleName`.

What I was suggesting was that `from_relative_path` should not be implemented like this as a `ModuleName` constructor, because then we throw away information from the path that we will need. I think `from_relative_path` should instead be implemented at a higher level where it returns all of the data that we can glean from the path, including the `ModuleName`.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:180 on 2024-04-19 14:56_

Yes, makes sense -- there are orders of magnitude more symbols and AST nodes than there are modules, so it's more important to maximize efficiency in those cases.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:315 on 2024-04-19 14:59_

Ah, I see. Yeah I don't think we should support resolving packages by their directory name.

For packages with an `__init__.py` I definitely don't think it makes sense.

The only case I can think of where it might make sense is a namespace package, but I don't think we need to (or should) ever resolve those directly or store them, because they cannot have contents. The only thing we care about is their submodules, which are resolved by their own path.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:423 on 2024-04-19 18:50_

Re the first comment, I'm not totally clear which are the cases where we would cache the content of a file but not also cache the resolution from module name to that file.

The idea that we may end up having module information without the source text due to cache eviction makes sense.

---

_@carljm reviewed on 2024-04-19 18:50_

---

_@carljm reviewed on 2024-04-19 18:52_

---

_Review comment by @carljm on `crates/red_knot/src/ast_ids.rs`:30 on 2024-04-19 18:52_

Oh weird! So basically it's just that Rust knows how to implement `Eq` for `fn() -> N` but not necessarily for `N`?

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:818 on 2024-04-19 18:57_

nit
```suggestion
        //       __init__.py
        //       one.py
```

---

_@carljm reviewed on 2024-04-19 18:58_

---

_@MichaReiser reviewed on 2024-04-19 21:31_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:54 on 2024-04-19 21:31_

I don't see us throwing away any information, considering that this method doesn't do any Io. The idea here is that we get a full qualified name that we can then throw into the resolve function (that retrieves all information)

---

_@carljm reviewed on 2024-04-19 23:50_

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:54 on 2024-04-19 23:50_

Ah, yeah, I was looking at this method in isolation; I see the only place it's actually used is in `resolve_path`, where the very next step is to resolve that module name back to a path (and make sure it actually resolves to the right path). So I agree, we will get all that information from resolving, and that's where we should get it from.

I would still prefer for this to be a private method of `ModuleResolver` rather than a public constructor of `ModuleName`, because I think it's kind of important to ensure that it only be used in the context of `resolve_path`, as just described. We don't want some code in the future constructing `ModuleName` using `ModuleName::from_relative_path` and assuming that means it has a correct module name to path correspondence. But this is more a future thing for robustness, not an important prototype consideration.

---

_@carljm reviewed on 2024-04-19 23:53_

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:374 on 2024-04-19 23:53_

Ok, I buy that we will need to be able to resolve modules without necessarily reading their contents.

It sounds like you are already planning that in future we should ensure that all filesystem access happens through VFS; in that case, we are on the same page. E.g. I want to be able to write tests for the logic of `ModuleResolver` using a mock VFS that don't require actual filesystem access.

---

_@MichaReiser reviewed on 2024-04-20 14:24_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:54 on 2024-04-20 14:24_

Yeah I don't mind making it private and agree, the only two methods that need to be public are `relative` and `new` (or absolute).

---

_Comment by @JonathanPlasse on 2024-04-22 06:25_

> https://excalidraw.com/#json=-Thvh6hnezji3DT3SfFYs,Hjt_fOpRTgpgNKy9Hfb9-Q

The link does not seem to be public.

---

_Comment by @MichaReiser on 2024-04-22 06:34_

@JonathanPlasse I just opened it in a private session without any problems. Do you get an error that the link is invalid? Also, the link is a bit outdated.

---

_Comment by @trag1c on 2024-04-22 06:47_

I can open it just fine :+1:

---

_Comment by @JonathanPlasse on 2024-04-22 13:35_

It works for me too in private session. Sorry for the noise.

---

_@MichaReiser reviewed on 2024-04-23 09:51_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:9 on 2024-04-23 09:51_

Nit :It's common in the rust community to name this `ty`

---

_@MichaReiser reviewed on 2024-04-23 09:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:54 on 2024-04-23 09:52_

Nit: I would rename this to `ClassType` because it's not a type class (a subset of a class) but instead it's the type representation of a class. 

Same for `TypeFunction`, rename to `FunctionType`. I think that also reads better with `LiteralType` rather than `TypeLiteral` (I think a type literal is something else, if it even exists).

---

_@MichaReiser reviewed on 2024-04-23 09:53_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:17 on 2024-04-23 09:53_

I think it will be interesting if we can come up with a more stable id for types than relying on inference order. 

---

_@carljm reviewed on 2024-04-23 18:24_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:17 on 2024-04-23 18:24_

Yeah, it's a good point, I thought about this. I don't think `TypeId` (as in, the index into this `IndexVec`) can be stable; that's not really compatible with lazy type evaluation. But we may want to have another, more stable identifier (based on fully qualified name?) that we use most places instead of `TypeId`.

---

_@carljm reviewed on 2024-04-23 18:25_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:54 on 2024-04-23 18:25_

Yeah, I agree. I did this in parallel with how AST names its types, e.g. `Stmt::ClassDef` contains a `StmtClassDef`. But I don't like how it works in this case, I prefer `Type` as suffix also.

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:17 on 2024-04-23 18:34_

I could also abandon using the `IndexVec` arena in this case? But we will have a lot of types...

---

_@carljm reviewed on 2024-04-23 18:34_

---

_Comment by @github-actions[bot] on 2024-04-23 19:24_

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

_Label `internal` added by @MichaReiser on 2024-04-27 08:26_

---

_Marked ready for review by @MichaReiser on 2024-04-27 08:27_

---

_Renamed from "Red Knot" to "Kick off Red-knot" by @MichaReiser on 2024-04-27 08:27_

---

_Merged by @MichaReiser on 2024-04-27 08:34_

---

_Closed by @MichaReiser on 2024-04-27 08:34_

---

_Branch deleted on 2024-04-27 08:34_

---
