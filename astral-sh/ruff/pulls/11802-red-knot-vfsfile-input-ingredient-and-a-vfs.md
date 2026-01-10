```yaml
number: 11802
title: "red-knot: `VfsFile` input ingredient and a `Vfs`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-files-vfs
created_at: 2024-06-08T15:22:33Z
updated_at: 2024-06-12T15:50:55Z
url: https://github.com/astral-sh/ruff/pull/11802
synced_at: 2026-01-10T21:56:00Z
```

# red-knot: `VfsFile` input ingredient and a `Vfs`

---

_Pull request opened by @MichaReiser on 2024-06-08 15:22_

This PR adds the foundation for red-knots salsa integration. It starts by defining a virtual filesystem that is Salsa's view of the files on disk (metadata only for now, I'll add support for content in the next PR).

### VFS

The `Vfs` supports operating on files from the local filesystem (disk, editor, WASM, in memory) or vendored stub files (this PR only adds a stub implementation). It is virtual because it supports multiple sources and it is Salsa's view of the file system state. 

The most notable change in terms of what we discussed for vendored files is that this PR exposes two methods on the `Vfs`:

* `file`: To "intern" a file system path. 
* `vendored`: To intern a vendored path


The motivation behind this is that the methods differ in their return-type (and argument, but that's less important). For `file`, it's important that the implementation returns a `File` object even for files that don't exist so that salsa can track the fact that this query needs to be re-executed if such a file gets created later. This isn't necessary for vendored files because the vendored file system is readonly. Returning `None` in that case reduces the number of dependencies that salsa has to track, and in turn, can result in better performance. Splitting the methods also has the benefit that they're easier to call. I suspect that code paths will either exclusively work with file system or vendored path. It would be cumbersome if the callers need to wrap the path in a `VfsPath` just for the interning.


### `FileSystem`

This PR also introduces a new `FileSystem` trait that abstracts away how filesystem files are read. The goal of this is that we can support different environments:
* WASM: Ruff might not have access to a full file system or it's all in memory as it is the case in our playground today
* LSP: The lsp does support reading from files, but unsaved changes take precedence over the content on disk. 
* tests: Ideally, tests don't need to write the content to disk. Instead, they can use an in-memory file system. 


### Crate name

I ended up creating a new crate because I couldn't find an existing crate that fits well. I first wanted to use `ruff_source_file` which is very similar. However, `ruff_notebook` depends on it and I suspect that this crate might depend on `ruff_notebook` in the long term (unless we use some `Arc<dyn Trait + Eq>` to represent file content). 

The other reason why I didn't put it in `ruff_source_file` is that it is intended to store low level data structures for files. A Vfs and a dependency on salsa feels a bit overkill for such a crate

### Open Questions

* dir walking: It's not entirely clear to me how to implement dir walking because `walkdir` doesn't support virtual filesystems. Ideally, both the memory file system and the real implementation would use the same dir walking mechanism to avoid differences in behavior. 
* File watching: I think file systems need either to automatically watch files for changes or need an API that allows setting up file watchers. They probably need a second API that allows pulling all changes (file added, removed, changed events). The host would call a `apply_changes` method that takes a mutable self whenever it observed file system changes, so that the `Vfs` can update its metadata.



---

_Label `red-knot` added by @MichaReiser on 2024-06-08 15:22_

---

_Renamed from "red-knot: First draft of the `File` input ingredient and a `Vfs`" to "red-knot: `File` input ingredient and a `Vfs`" by @MichaReiser on 2024-06-08 15:22_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs/path.rs`:99 on 2024-06-08 15:23_

@AlexWaygood I think this could be interesting for you. Ideally, `Vendored` would wrap a `VendoredPathBuf` but I decided not to do this as part of this PR.

---

_@MichaReiser reviewed on 2024-06-08 15:23_

---

_@MichaReiser reviewed on 2024-06-08 15:24_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:151 on 2024-06-08 15:24_

@AlexWaygood what I have in mind here is that the implementation calls into the `vendored` crate to read the file metadata (`vendored_path.last_modification_time()`, `vendored_path.exists()`, and `ruff_venored::read_to_string(vendored_path)`?)

---

_Comment by @github-actions[bot] on 2024-06-08 15:52_

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

_Renamed from "red-knot: `File` input ingredient and a `Vfs`" to "red-knot: `VfsFile` input ingredient and a `Vfs`" by @MichaReiser on 2024-06-09 08:14_

---

_@MichaReiser reviewed on 2024-06-09 13:55_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/lib.rs`:23 on 2024-06-09 13:55_

@AlexWaygood I think we ultimately want two different entry functions for regular files, and vendored files. Mainly because I think it's a bit annoying if you're working with a vendored path and you then need to convert it to a `VfsPath` just to get the `VfsFile`. Funnily, the first thing that the implementation would do is to dispatch on the path type.

---

_Review requested from @carljm by @MichaReiser on 2024-06-10 06:30_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-10 06:30_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:34 on 2024-06-10 06:32_

CC: @snowsignal I don't plan to support this as part of this PR but something we need to figure out for red-knot/LSP

---

_@MichaReiser reviewed on 2024-06-10 06:32_

---

_Marked ready for review by @MichaReiser on 2024-06-10 06:32_

---

_Comment by @codspeed-hq[bot] on 2024-06-10 14:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/salsa-files-vfs)

### Merging #11802 will **not alter performance**

<sub>Comparing <code>salsa-files-vfs</code> (ce952b3) with <code>salsa-files-vfs</code> (a98db3d)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:14 on 2024-06-10 15:53_

This might be something of a Rust-newbie question: what's the advantage of defining a public type alias here instead of just doing `use std::io::Result;`?

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:18 on 2024-06-10 15:59_

```suggestion
/// The file system is agnostic to the actual storage medium: it could be a real file system, a combination
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:30 on 2024-06-10 16:06_

Did you consider using e.g. https://docs.rs/vfs/latest/vfs/filesystem/trait.FileSystem.html# rather than creating a new trait? I suppose advantages of creating our own are (1) we don't need to implement all the required methods that that trait has and (2) it enables us to avoid adding another external dependency?

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:164 on 2024-06-10 16:27_

Same question as https://github.com/astral-sh/ruff/pull/11338/files#r1633165461 -- what do we need to know the permissions of the file for? If the file doesn't have read permissions, I think for our purposes we might as well model that as the file just not existing. And I don't think we'd need any other permissions, would we?

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:38 on 2024-06-10 16:29_

Does this mean that we won't provide type-checking for Python scripts if their filename contains non-utf8 characters? Is this a limit Ruff already has when linting Python?

I think we can guarantee that vendored files are always going to be valid utf-8 but I'm not sure about non-vendored files

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:250 on 2024-06-10 16:55_

bah, so verbose, could do this in one line with a lil' `is_macro` dependency ;)

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/memory.rs`:12 on 2024-06-10 16:55_

```suggestion
/// In-memory file system.
```

---

_Comment by @MichaReiser on 2024-06-10 16:55_

Okay, not having a `db.file(VfsPath)` method is going to be problematic. I realized this when implementing the module resolver where we have a `path_to_module(db, VfsFilePath)` function. I could match on the `path` and call the two different functions but that's rather annoying. 

I have to figure out how we handle the signature difference between vendored paths and regular paths. I would very much like the possibility that `vendored` returns `None` if the file doesn't exist. 

I plan to address this as a separate PR to reduce my rebasing work.

Edit: This is solved in https://github.com/astral-sh/ruff/pull/11826
 

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:164 on 2024-06-10 16:58_

Oh, I suppose we need to know whether or not we have write permissions if we're going to be able to do autofixes or fixup formatting. I forget that `red-knot` has broader ambitions than just type-checking 😄

Still, I wonder if this really needs to be `Option<u32>` or if we could just have a `writable: bool` field?

---

_Review comment by @snowsignal on `crates/ruff_db/src/file_system.rs`:34 on 2024-06-10 16:59_

Thanks for pinging me on this!

---

_@snowsignal reviewed on 2024-06-10 16:59_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:14 on 2024-06-10 17:06_

I see in `crates/ruff_db/src/file_system/memory.rs` we have to use `std::io::Error::new()` when returning error types, which makes me think even more that we may as well just use `std::io::Result` here directly

---

_Renamed from "red-knot: `VfsFile` input ingredient and a `Vfs`" to "red-knot: `VfsFile` input ingredient and a `Vfs` [salsa 1..]" by @MichaReiser on 2024-06-11 13:40_

---

_Renamed from "red-knot: `VfsFile` input ingredient and a `Vfs` [salsa 1..]" to "red-knot(salsa part 1): `VfsFile` input ingredient and a `Vfs`" by @MichaReiser on 2024-06-11 13:40_

---

_Renamed from "red-knot(salsa part 1): `VfsFile` input ingredient and a `Vfs`" to "red-knot[salsa part 1]: `VfsFile` input ingredient and a `Vfs`" by @MichaReiser on 2024-06-11 13:42_

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:39 on 2024-06-11 15:03_

I guess we need this for the safety of the pointer-casting below in `new`?

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:48 on 2024-06-11 15:06_

Maybe it seems trivial here, but best to have a SAFETY comment? (And mention the `repr(transparent)` above)

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:41 on 2024-06-11 15:07_

At the moment this type seems to be purely a transparent wrapper around a `camino::Utf8Path`. Do we expect it to do more in the future? Or does it exist solely for type-system purposes (as a sort of newtype), so we can control creation of our vfs paths?

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:85 on 2024-06-11 15:22_

yeah this is exactly the comment that I thought should also be in `FileSystemPath::new` above

actually, could we just call `FileSystemPath::new` here instead of duplicating the same unsafe block?

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:95 on 2024-06-11 15:25_

curious what your heuristic is for deciding that this one should not be marked `#[inline]`

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:159 on 2024-06-11 15:26_

These can't just be derived?

---

_Review comment by @carljm on `crates/ruff_db/src/file_system.rs`:223 on 2024-06-11 15:29_

how do you choose between `u128::from(...)` vs `.. as u128`, here vs above?

---

_Review comment by @carljm on `crates/ruff_db/src/lib.rs`:17 on 2024-06-11 15:35_

Cupboard is a cute naming idea, but if a) salsa doesn't use it, and b) we aren't going to fully commit to it (using it throughout our own code), then I think mentioning it in a comment is just confusing

---

_Review comment by @carljm on `crates/ruff_db/src/vfs.rs`:187 on 2024-06-11 15:46_

0 or `None`?

Should this be an `Option<NonZeroU32>`?

---

_Review comment by @carljm on `crates/ruff_db/src/vfs/path.rs`:17 on 2024-06-11 15:51_

SAFETY comment?

---

_Review comment by @carljm on `crates/ruff_db/src/vfs.rs`:31 on 2024-06-11 16:00_

I do find this layering pretty confusing in trying to understand the code. I think partly it's just the naming: what we call `FileSystem` is already "virtual" in that it can represent on-disk, memory, etc. And then we have another "virtual file system" layer on top of that.

I'm also not sure of the accuracy of this statement:

> The other reason is that most operations know if they are working with vendored or file system paths.

Maybe it will turn out to be the case? It's just not clear to me.

---

_@carljm approved on 2024-06-11 17:15_

Strong +1 on the decision to make this its own crate. I think in general making new crates for red-knot functionality should be preferred over squeezing it into existing crates; I think the latter will cause more confusion. In particular I don't think we should try to put red-knot's semantic model into the existing `ruff_python_semantic` crate.

---

_@MichaReiser reviewed on 2024-06-11 18:38_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:159 on 2024-06-11 18:38_

Display can't be derived and the default Debug would print FileSystemPath(path) which seems unnecessary 

---

_Comment by @MichaReiser on 2024-06-11 18:44_

> Strong +1 on the decision to make this its own crate. I think in general making new crates for red-knot functionality should be preferred over squeezing it into existing crates; I think the latter will cause more confusion. In particular I don't think we should try to put red-knot's semantic model into the existing ruff_python_semantic crate.

I'm a bit surprised by this comment because that's what I proposed in discord and I didn't see any objection to doing this for ruff_python_semantic

I can see how it can cause confusion. Mainly for imports when you get both Symbols (the ruff and red_knot one). 

I don't think we can't avoid mixing them long term. The red knot crates at least will have to depend on their corresponding duff crates to use their functionality and it then becomes less clear to me what the benefit of keeping them separate really is. I also think that it is a motivation function to integrate early and more often compared to building this out entirely in different crates 

---

_@MichaReiser reviewed on 2024-06-11 18:47_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:31 on 2024-06-11 18:47_

I can say from using the API that it's working pretty well so far. But I admit that the terminology is confusing. Do you have any suggestions on how the naming could be improved or what specifically you find confusing.

I can try to improve the documentation. The way I think about FileSystem is less that it is a virtual file system. It's rather more how we access it. Mwhat would you think of FileSystemDriver?

---

_@MichaReiser reviewed on 2024-06-11 18:50_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:41 on 2024-06-11 18:50_

There are multiple goals

- To prevent that FileSystemPath and VendoredPath are mixed
- To remove all methods that perform IO to force us to use the file system API (eg path.exists)
- To make it easier to change the internal representation. We'll have to figure out how to represent untitled files in the editor (they have no path). It's quite likely that we'll use just plain strings internally 


---

_@MichaReiser reviewed on 2024-06-11 18:51_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:39 on 2024-06-11 18:51_

Yeah exactly. Otherwise the Path and a reference to a PathBuf aren't guaranteed to have the same layout 

---

_@carljm reviewed on 2024-06-12 01:07_

---

_Review comment by @carljm on `crates/ruff_db/src/vfs.rs`:31 on 2024-06-12 01:07_

I spent some more time looking at it and I think it's fine, if it seems to you to be working out well. I'm not sure that a rename of `FileSystem` to `FileSystemDriver` would make much difference; I think "Driver" ends up being kind of an empty filler word there that doesn't really communicate much.

It was hard for me to figure out the model for how these two things interact, since neither one encapsulates the other. It seems the model is that a Db has both a Vfs and a FileSystem, and it will use the FileSystem for looking up filesystem paths, and manage looking up vendored paths itself, though that hasn't been implemented yet, just stubbed.

---

_Comment by @carljm on 2024-06-12 04:22_

> I'm a bit surprised by this comment because that's what I proposed in discord and I didn't see any objection to doing this for ruff_python_semantic

Oh sorry! I think I do remember that Discord comment now; I guess I didn't think very carefully about it then.

> I can see how it can cause confusion. Mainly for imports when you get both Symbols (the ruff and red_knot one).

I think it will just be generally hard to figure out how to structure things within one crate to make it clear what is red-knot and what is "v1". E.g. there is a top-level `definition.rs` in `ruff_python_semantic`, but red-knot will have its own version of Definitions; where do they go? Perhaps it will help me envision how this can work if I see the overall structure you have in mind within the crate to avoid just having a random assortment of sub-modules, some of which are v1 and some of which are red-knot, with no obvious way to tell which is which. If this structure is the outcome, I don't think that's a good outcome

> I don't think we can't avoid mixing them long term. The red knot crates at least will have to depend on their corresponding duff crates to use their functionality and it then becomes less clear to me what the benefit of keeping them separate really is. I also think that it is a motivation function to integrate early and more often compared to building this out entirely in different crates

I agree that red knot will probably have to depend on v1, but the dependency should not go in the other direction, and to me this is enough reason to keep them separate.

I'm not clear what sort of "integration" we actually envision between the v1 semantic model and the red-knot semantic model that this would encourage. Can you clarify what kind of integration you are thinking of? My understanding is that they will always remain separate, and rules will have to explicitly be ported in some way.

---

_Comment by @MichaReiser on 2024-06-12 06:02_

> I think it will just be generally hard to figure out how to structure things within one crate to make it clear what is red-knot and what is "v1". E.g. there is a top-level definition.rs in ruff_python_semantic, but red-knot will have its own version of Definitions; where do they go? Perhaps it will help me envision how this can work if I see the overall structure you have in mind within the crate to avoid just having a random assortment of sub-modules, some of which are v1 and some of which are red-knot, with no obvious way to tell which is which. If this structure is the outcome, I don't think that's a good outcome

I agree on this and something I have started thinking about as well. I don't think what I've done in my follow up PRs is ideal and I wanted to iterate on it. For now, the red knot code is gated behind the `red_knot` feature. That means, by default the `ruff_python_semantic` crate doesn't compile with any red knot functionality. This prevents that any `v1` code access `red_knot` code from ruff. 

I think what could be improved is that we move all `red_knot` code that isn't shared between `v1` and `red_knot` into a `red_knot` module. Although it then quickly becomes unclear what should be in there. Should we move the `db` out as soon as a single `v1` API uses any red knot code? What about code that doesn't exist in `v1` at all, e.g. the module resolver?

Regardless, this would then be very close to having two separate crates. The difference I see is that moving files in a crate tends to be easier and we can also already make use of the right crate visibilities (we don't need to make anything `pub` just so that we can access it from the `red_knot` crate. 

I overall don't have a strong preference and I think I would just go with one approach for now. We don't need to get it right just now, it's easy to split the code out later.

> I'm not clear what sort of "integration" we actually envision between the v1 semantic model and the red-knot semantic model that this would encourage. Can you clarify what kind of integration you are thinking of? My understanding is that they will always remain separate, and rules will have to explicitly be ported in some way.

I at least want to try to come up with a rule API that would work for both `v1` and `red_knot`, at least for a majority of the rules. I would strongly prefer if we can avoid copying rules from `ruff` to the `red_knot` crate because that would come close to a rule-freeze for a couple of months. But this requires that `ruff_linter` has access to both `red_knot` and `v1` code.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:31 on 2024-06-12 06:35_

Maybe what's confusing here is the naming of `Vfs`? Maybe it should just be `VfsFiles`? 

Anyway, I do think your concern is valid and I went back and forth a couple of times between `FileSystem` handling all `VfsPath`s and the `FileSystem` only handling specific paths. And maybe my reasoning that vendored paths are different enough to not pass them through `FileSystem` is unjustified, considering that calling `write_file` with a path pointing to a directory also fails. So there's anyway some extra care that needs to be taken when handling paths that aren't checked at compile time. But I find it kind of nice if we can catch some of them at compile time.

The way I think about it is that `FileSystem` is a replacement for `std::fs`, that's it. It doesn't add support for using multiple file systems at once (vendored and the regular one), which is what `Vfs` does. 

An entirely different design (not fully thought through) would be to ditch the `FileSystem` trait all together. The main motivation for it is that we can support unsaved files and untitled files in the LSP use case. But we could support this in another way by adding a `open_file` and `close_file` function to `Vfs` that allows to manually override the content of a file. 

The only functionality we would looe is that we can't "mock out" the file system during tests with an in memory file system. But maybe that's not worth going through all that trouble. The WASM integration story would then require to provide a "stub" FS implementation. It's not a big detail and something that e.g. emscripten provides out of the box but it comes with a bit more boilerplate code than an option to just use a memory file system. 

Either way, I don't consider the Vfs / FileSystem design that I proposed here as the final design. It requires some more iterating and I'm open to redesigning it completely. 

* We merge what we have now, with the risk that we might need to refactor some of the calling code
* We strip out the `FileSystem` trait and revisit the design when implementing LSP support.

I'm leaning towards keeping what we have here. There's not a lot of downstream code depending on `FileSystem`, so that a refactor should be fairly painless. 




---

_@MichaReiser reviewed on 2024-06-12 06:35_

---

_@MichaReiser reviewed on 2024-06-12 06:50_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:95 on 2024-06-12 06:50_

Probably most methods in here should be marked as `inline`. Not that I think it matters too much for our use case. Inline tells the Rust compiler to emit sufficient information that cross-crate inlining works even if LTO is disabled. That's less important for us because we have LTO (thin) enabled. 

My general thinking is: If the method is only a very thin wrapper, that is called frequently add `[#inline]`. But I'm not super consistent on it as you can see


The call in the test needs to use `as` because the lossy conversion there is intentional.

---

_@MichaReiser reviewed on 2024-06-12 06:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:223 on 2024-06-12 06:53_

Good catch. I should probably use `from` here too. 

> When converting between numeric types, one thing to note is that From is only implemented for lossless conversions (e.g. you can convert from i32 to i64 with From, but not the other way around), whereas as works for both lossless and lossy conversions (if the conversion is lossy, it truncates). Thus, if you want to ensure that you don't accidentally perform a lossy conversion, you may prefer using From::from rather than as. [source](https://stackoverflow.com/questions/48795329/what-is-the-difference-between-fromfrom-and-as-in-rust)

---

_@MichaReiser reviewed on 2024-06-12 06:57_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:187 on 2024-06-12 06:57_

It should be `None`. 

The FS api uses a `u32` as return type. I'm not aware that 0 is ever a valid permission but I want to avoid conversion errors and using a `NonZeroU32` doesn't give us much here (other than the size savings)

---

_Comment by @MichaReiser on 2024-06-12 07:01_

I'll go ahead and merge this PR. This does not mean that we made a decision on how to proceed with `FileSystem` and `Vfs` or where we want to place the `red_knot` code in `ruff_python_semantic`. I just don't consider these two decisions as merge blocking and I would prefer to do this refactor as separate PRs on top of my entire stack anyway, because I don't enjoy suffering through rebases.

---

_@MichaReiser reviewed on 2024-06-12 07:03_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:223 on 2024-06-12 07:03_

Actually, I have to use `as` for seconds because seconds is a `i64` and converting it to an `u128` doesn't suffer truncation, but the conversion isn't lossless.

---

_Renamed from "red-knot[salsa part 1]: `VfsFile` input ingredient and a `Vfs`" to "red-knot: `VfsFile` input ingredient and a `Vfs`" by @MichaReiser on 2024-06-12 07:03_

---

_Merged by @MichaReiser on 2024-06-12 07:06_

---

_Closed by @MichaReiser on 2024-06-12 07:06_

---

_Branch deleted on 2024-06-12 07:06_

---

_Comment by @carljm on 2024-06-12 13:27_

I do see that visibilities could be a reason to share a crate, if there's a lot of use of "old" internals from red-knot code.

Ultimately I think sharing a crate could work fine, too, as long as we have a structure that makes it clear what is v1 and what is red-knot.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:41 on 2024-06-12 15:30_

```suggestion
    /// Lookup table that maps VfsPaths to salsa-interned [`VfsFile`] instances.
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:96 on 2024-06-12 15:32_

```suggestion
    /// Looks up a vendored file by its path. Returns `Some` if a vendored file for the given path
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:147 on 2024-06-12 15:33_

```suggestion
    /// Creates a salsa-like snapshot of the files. The instances share
    /// the same path-to-file mapping.
```

---

_@AlexWaygood reviewed on 2024-06-12 15:44_

Sorry for the slow review here. It took me a little bit of time to get my head round some of this, but it LGTM. Happy to open my own followup PR for some of my docs nits, unless there are any you particularly disagree with!

---

_@MichaReiser reviewed on 2024-06-12 15:47_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:14 on 2024-06-12 15:47_

It makes it easier to change the result type later, because I made sure that all methods return `file_system::Result`. It's also easier to write because I don't have to think about what's the right return type. I can just use that.

---

_@AlexWaygood reviewed on 2024-06-12 15:48_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:14 on 2024-06-12 15:48_

Makes sense, thank you.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:30 on 2024-06-12 15:49_

I considered it but it implements more functionality than we need and lacks functionality that we want. For example, we need a way to walk the file system, ideally I want to use `walk_dir` for that. I also want to add file watching functionality that `Vfs` lacks. We could obviously extend the `Vfs` trait, but it just didn't feel like a great fit.

---

_@MichaReiser reviewed on 2024-06-12 15:49_

---

_@MichaReiser reviewed on 2024-06-12 15:49_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:164 on 2024-06-12 15:49_

Some ruff rules check the file permissions. 

---

_@AlexWaygood reviewed on 2024-06-12 15:50_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:164 on 2024-06-12 15:50_

TIL

---

_@MichaReiser reviewed on 2024-06-12 15:50_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:38 on 2024-06-12 15:50_

Yes, it limits us to UTF8 paths only but Ruff already assumes UTF8 today (we have so many `path.to_str().unwrap()` calls. Also, non UTF8 paths are extremely uncommon. I think all modern file system support UTF8 today (or at least, paths can be encoded to UTF8).

---

_@MichaReiser reviewed on 2024-06-12 15:50_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:250 on 2024-06-12 15:50_

no comment

---
