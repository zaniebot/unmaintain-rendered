```yaml
number: 12494
title: "[red-knot] Refactor `path.rs` in the module resolver"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/refactor-path
created_at: 2024-07-24T16:34:47Z
updated_at: 2024-07-25T18:45:06Z
url: https://github.com/astral-sh/ruff/pull/12494
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Refactor `path.rs` in the module resolver

---

_@AlexWaygood_

## Summary

This PR more-or-less completely rewrites the `path.rs` submodule in the redknot module resolver, addressing @carljm's comments in https://github.com/astral-sh/ruff/pull/12141#discussion_r1667010245 and https://github.com/astral-sh/ruff/pull/12141#discussion_r1667031874. The external API exposed to other submodules in the module resolver is almost entirely unchanged, however.

Some of the major changes made include:
- There's no longer two enums, one representing owned module paths and one representing borrowed module paths. Instead, there's a single `ModulePath` type.
- The `ModuleSearchPath` type in `path.rs` (renamed to `SearchPath`) no longer dereferences to `ModulePath`. Instead, it's a fully distinct type that only implements the methods that make sense on a search path.
- Custom stdlib paths and vendored stdlib paths are now represented as two different variants. A custom stdlib path is in many ways more similar to a site-packages path than it is to a vendored stdlib path. Treating them as two separate variants cleans up the code a bunch, and allows us to remove any usage of the `FilePath` enum from `path.rs`.
- `ModulePath`s now always remember the search path from which they were derived, and are always represented internally as a `{search_path, relative_path}` struct.

## Test Plan

`cargo test -p red_knot_module_resolver`


---

_Label `red-knot` added by @AlexWaygood on 2024-07-24 16:34_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-24 16:34_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-24 16:34_

---

_Comment by @github-actions[bot] on 2024-07-24 16:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-07-24 18:59_

I like the direction this is going but still think it's more complicated than it needs to be and the `'static` lifetime is very clever, but too clever in my view (it's hard to understand). 

I think `ModuleSearchPath` could be a simple struct that doesn't use any generics

```rust
#[derive(Clone, Debug, Eq, PartialEq)]
pub struct ModuleSearchPath {
	kind: ModuleSearchPathKind,
	path: Utf8PathBuf
}

#[derive(Copy, Clone, Debug, Eq, PartialEq)]
pub enum ModuleSearchPathKind {
	Extra,
    FirstParty,
	StandardLibraryCustom,
	StandardLibraryVendored,
	SitePackages,
	Editable,
}
```

Using a `Utf8PathBuf` for both vendored and system paths removes the generic aspect and should be sufficient because we can use the `kind` to determine whether we should construct ` vendored` or `system` path. 

Decoupling the `Path` and `Kind` has the added benefit that we can remove the matching on `kind` for most mutation methods **unless** the logic depends on the kind. 


I very much do like `ModuleSearchPath` and that it is its own type. 


---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:912 on 2024-07-24 19:01_

Nit

```suggestion
        match &self.0 {
            ModuleSearchPathInner::StandardLibraryVendored(path) => Some(&*path.0),
            ModuleSearchPathInner::Extra(_) |
            ModuleSearchPathInner::FirstParty(_) |
            ModuleSearchPathInner::StandardLibraryCustom(_) |
            ModuleSearchPathInner::SitePackages(_) |
            ModuleSearchPathInner::Editable(_) => None,
        }
    }
```

---

_@MichaReiser reviewed on 2024-07-24 19:01_

---

_@MichaReiser reviewed on 2024-07-24 19:01_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:899 on 2024-07-24 19:01_

Nit
```suggestion
        match &self.0 {
            ModuleSearchPathInner::Extra(path) |
            ModuleSearchPathInner::FirstParty(path) |
            ModuleSearchPathInner::StandardLibraryCustom(path) |
            ModuleSearchPathInner::SitePackages(path) |
            ModuleSearchPathInner::Editable(path) => Some(&*path.0),
            ModuleSearchPathInner::StandardLibraryVendored(_) => None,
        }
```

---

_@carljm approved on 2024-07-25 03:19_

This looks really good to me, as far as the type changes for module and search paths! Thanks for doing this refactor.

I don't have a strong opinion either way about the static lifetime thing, so I'll leave this for Micha to approve when he's happy with it.

---

_Comment by @carljm on 2024-07-25 03:22_

Oh, when reading this, like Micha I also thought about decoupling the path from the kind, instead of wrapping the path inside the kind enum, so that we could do fewer pointless matches on kind where every arm does the same thing. If we could store just a `Utf8PathBuf` and have the kind imply vendored vs system, I think that would likely simplify this even further.

---

_Comment by @AlexWaygood on 2024-07-25 10:16_

I pushed a commit simplifying the situation with the `'static` lifetimes. Methods that mutate internal state are now always available on `ModulePath`, even if the lifetime is not `'static`; copy-on-write semantics are now just an implementation detail of the internal structure of `ModulePath`. (This shouldn't result in any extra allocation.)

> Using a `Utf8PathBuf` for both vendored and system paths removes the generic aspect and should be sufficient because we can use the `kind` to determine whether we should construct ` vendored` or `system` path.
> 
> Decoupling the `Path` and `Kind` has the added benefit that we can remove the matching on `kind` for most mutation methods **unless** the logic depends on the kind.

It sounds like I'm in the minority here, but I prefer wrapping the data inside the enum that determines the kind of path we're dealing with. I think it leaves less room for error: it forces you to consider what kind of variant you're dealing with before you're allowed to access the underlying data and manipulate it. Otherwise it feels too easy to me to forget to write a method that manipulates the data and forgets to consider the kind. I agree the way I currently have it is more verbose, but I feel like it leaves less room for error.

---

_Comment by @AlexWaygood on 2024-07-25 10:21_

> that doesn't use any generics

Rather than having generic `ModulePathData` and `SearchPathData` structs, I considered having separate `SystemModulePathData`, `VendoredModulePathData`, `SystemSearchPathData` and `VendoredSearchPathData` structs. It would be a bit less DRY, but it would avoid having to use the complex generic parameters and trait bounds. Would you consider that an improvement?

---

_Comment by @MichaReiser on 2024-07-25 10:26_

> Rather than having generic ModulePathData and SearchPathData structs, I considered having separate SystemModulePathData, VendoredModulePathData, SystemSearchPathData and VendoredSearchPathData structs. It would be a bit less DRY, but it would avoid having to use the complex generic parameters and trait bounds. Would you consider that an improvement?

I don't think I fully understand the change but I have some reservations about the `Data` structs. I do find that they introduce a lot of boilerplate and indirection, which makes it more difficult to understand the essence of the code (I actually think that it overoptimizes on isolation). To me it would also be unclear how it would allow you to separate `path` and `kind`, which both Carl and I think would simplify many mapping/mutation methods.

---

_Comment by @AlexWaygood on 2024-07-25 10:30_

> To me it would also be unclear how it would allow you to separate `path` and `kind`, which both Carl and I think would simplify many mapping/mutation methods.

Yeah, it wouldn't separate `path` and `kind`. As I said in https://github.com/astral-sh/ruff/pull/12494#issuecomment-2249979713, I agree that that would be a simplification but I have reservations about making that change.

---

_Comment by @AlexWaygood on 2024-07-25 11:12_

Getting rid of the generics did simplify the code a fair bit, so I've pushed that change. The PR now doesn't use generics at all: `SystemModulePathData` and `VendoredModulePathData` are just two separate structs, rather than there being a single, generic `ModulePathData` struct.

---

_Comment by @MichaReiser on 2024-07-25 11:38_

I kind of disagree. More code tends to be more bugs. 

Considering that we're already storing the search path as an explicit field, why not store the `ModuleSearchPath` on `ModulePath`? You then can keep the explicit enum that forces checking that it the right variant when *dereferencing* the search path, but all operations on the relative path don't need to deal with the kind, unless they have to.

Why do I feel strongly about this. There are so many `Data` and module structs involved that I find it hard to understand what's going on and how they related to each other. Reducing the types will make it easier to understand the code.

I do think that it will allow you to reduce the code significantly (which is a main motivation for this refactor in the first place!)

---

_Comment by @AlexWaygood on 2024-07-25 11:50_

> Considering that we're already storing the search path as an explicit field, why not store the `ModuleSearchPath` on `ModulePath`? You then can keep the explicit enum that forces checking that it the right variant when _dereferencing_ the search path, but all operations on the relative path don't need to deal with the kind, unless they have to.

Just to make sure I understand correctly: you're proposing something like this? I'm happy to try that out, if so:

```rs
enum SearchPath {
    Extra(Arc<SystemPathBuf>),
    FirstParty(Arc<SystemPathBuf>),
    StandardLibraryCustom(Arc<SystemPathBuf>),
    StandardLibraryVendored(Arc<VendoredPathBuf>),
    SitePackages(Arc<SystemPathBuf>),
    Editable(Arc<SystemPathBuf>),
}

struct ModulePath<'a> {
    search_path: SearchPath,
    relative_path: Cow<'a, Utf8Path>
}
```

---

_Comment by @MichaReiser on 2024-07-25 11:59_

Yes, that's what I'm thinking. Although I think we don't have to use a `Cow` for `relative_path` and can instead change `with_pyi_extension` to return `Self` and `with_pyi_extension` return a `Result<Self, Self>` and instead mutate the path in place. 

The place I would like to get is that for example for:

```rust
    #[must_use]
    pub(crate) fn is_directory(&self, resolver: &ResolverState) -> bool {
        match &self.0 {
            ModulePathInner::Extra(path) => {
                resolver.system().is_directory(&path.to_absolute_path_buf())
            }
            ModulePathInner::FirstParty(path) => {
                resolver.system().is_directory(&path.to_absolute_path_buf())
            }
            ModulePathInner::StandardLibraryCustom(path) => {
                match query_custom_stdlib_version(path, resolver) {
                    TypeshedVersionsQueryResult::DoesNotExist => false,
                    TypeshedVersionsQueryResult::Exists => {
                        resolver.system().is_directory(&path.to_absolute_path_buf())
                    }
                    TypeshedVersionsQueryResult::MaybeExists => {
                        resolver.system().is_directory(&path.to_absolute_path_buf())
                    }
                }
            }
            ModulePathInner::StandardLibraryVendored(path) => {
                match query_vendored_stdlib_version(path, resolver) {
                    TypeshedVersionsQueryResult::DoesNotExist => false,
                    TypeshedVersionsQueryResult::Exists => resolver
                        .vendored()
                        .is_directory(path.to_absolute_path_buf()),
                    TypeshedVersionsQueryResult::MaybeExists => resolver
                        .vendored()
                        .is_directory(path.to_absolute_path_buf()),
                }
            }
            ModulePathInner::SitePackages(path) => {
                resolver.system().is_directory(&path.to_absolute_path_buf())
            }
            ModulePathInner::Editable(path) => {
                resolver.system().is_directory(&path.to_absolute_path_buf())
            }
        }
    }
```

I as a reader find it extremely difficult to spot the difference between the branches. I have to read trough the entire cod and compare the code for each branch only to see, ah, most of them are the same. 

This method is similar

```rust
    #[must_use]
    pub(crate) fn to_module_path(&self) -> ModulePath<'static> {
        match &self.0 {
            ModuleSearchPathInner::Extra(search_path) => {
                ModulePath::extra(SystemModulePathData::from_search_path(search_path.clone()))
            }
            ModuleSearchPathInner::FirstParty(search_path) => {
                ModulePath::first_party(SystemModulePathData::from_search_path(search_path.clone()))
            }
            ModuleSearchPathInner::StandardLibraryCustom(search_path) => ModulePath::custom_stdlib(
                SystemModulePathData::from_search_path(search_path.clone()),
            ),
            ModuleSearchPathInner::StandardLibraryVendored(search_path) => {
                ModulePath::vendored_stdlib(VendoredModulePathData::from_search_path(
                    search_path.clone(),
                ))
            }
            ModuleSearchPathInner::SitePackages(search_path) => ModulePath::site_packages(
                SystemModulePathData::from_search_path(search_path.clone()),
            ),
            ModuleSearchPathInner::Editable(search_path) => {
                ModulePath::editable(SystemModulePathData::from_search_path(search_path.clone()))
            }
        }
    }
```

I think with what I suggested above, this could be reduced to `ModulePath::new(self.clone())` which makes it much easier to understand as a reader.

---

_Comment by @AlexWaygood on 2024-07-25 12:00_

Alright, I'll try making that change

---

_Comment by @AlexWaygood on 2024-07-25 12:32_

> I think we don't have to use a `Cow` for `relative_path`

I got rid of the `Cow`. It means we have to do an otherwise-unnecessary allocation in `relativize_path()`, but other than that the code is unchanged -- so you're probably right that it wasn't worth the added complexity. And `relativize_path()` is only used in `resolver::file_to_module`, which is only used in `resolver::path_to_module`, which is currently unused.

I'm looking at your larger suggested change now.

---

_Comment by @MichaReiser on 2024-07-25 13:07_

Thanks Alex for taking the time!

---

_Comment by @AlexWaygood on 2024-07-25 16:23_

I pushed the refactor. It made the code around 200 lines shorter and I don't think it makes the code significantly more bug-prone. Thanks for pushing me on this :-)

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-25 16:23_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:61 on 2024-07-25 16:27_

Nit: This methods between the struct declaration and the `impl` block are a bit distracting to the reading flow. Maybe move them further down?

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:73 on 2024-07-25 16:29_

I don't think it's worth changing the design because of it. But the fact that the `ModulePath` is constructed out of the search path and a relative path requires that calling `is_directory` always allocates a new path (hidden behind `join`)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:73 on 2024-07-25 16:30_

Actually, almost all operations that now operate on the full path will require an allocation.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:259 on 2024-07-25 16:31_

It's slightly surprising that `Eq` allocates. I think you could optimize this by doing 

```
if let Some(rest) = other.split_prefix(self.search_path) {
    rest == &self.relative_path
} else {
    false
}
```

Same for vendored path

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:507 on 2024-07-25 16:33_

I think it would be possible to merge some of these branches
```suggestion
        match (&*self.0, path) {
            (SearchPathInner::Extra(search_path), FilePath::System(absolute_path)) | (SearchPathInner::FirstParty(search_path), FilePath::System(absolute_path)) => absolute_path
                .strip_prefix(search_path)
                .ok()
                .map(|relative_path| ModulePath {
                    search_path: self.clone(),
                    relative_path: relative_path.as_utf8_path().to_path_buf(),
                }),
            (SearchPathInner::Extra(_), FilePath::Vendored(_)) => None,
            (SearchPathInner::FirstParty(_), FilePath::Vendored(_)) => None,
```

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:576 on 2024-07-25 16:33_

We should return a `&SystemPath` and `&VendoredPath` instead of the `buf` version
```suggestion
    pub(crate) fn as_system_path_buf(&self) -> Option<&SystemPath> {
        match &*self.0 {
            SearchPathInner::Extra(path)
            | SearchPathInner::FirstParty(path)
            | SearchPathInner::StandardLibraryCustom(path)
            | SearchPathInner::SitePackages(path)
            | SearchPathInner::Editable(path) => Some(path),
            SearchPathInner::StandardLibraryVendored(_) => None,
        }
    }

    #[must_use]
    pub(crate) fn as_vendored_path_buf(&self) -> Option<&VendoredPath> {
        match &*self.0 {
            SearchPathInner::StandardLibraryVendored(path) => Some(path),
            SearchPathInner::Extra(_)
            | SearchPathInner::FirstParty(_)
            | SearchPathInner::StandardLibraryCustom(_)
            | SearchPathInner::SitePackages(_)
            | SearchPathInner::Editable(_) => None,
        }
    }
}
```

---

_@MichaReiser approved on 2024-07-25 16:34_

Awesome!

---

_@AlexWaygood reviewed on 2024-07-25 17:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:576 on 2024-07-25 17:38_

> We should return a `&SystemPath` and `&VendoredPath` instead of the `buf` version

I don't mind making the change, but what's the advantage?

---

_@AlexWaygood reviewed on 2024-07-25 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:73 on 2024-07-25 17:42_

I suppose we could store module paths as a `{search_path, absolute_path}` struct rather than a `{search_path, relative_path}` struct. But then we'd probably have to think about encoding the type in the `absolute_path` part of the struct again...

---

_@AlexWaygood reviewed on 2024-07-25 18:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:507 on 2024-07-25 18:04_

Could do, but Clippy complains about ^that because it thinks I should use 

```rs
(
    SearchPathInner::Extra(search_path) | SearchPath::FirstParty(search_path),
    FilePath::System(absolute_path)
)
```

instead of

```rs
    (SearchPathInner::Extra(search_path), FilePath::System(absolute_path))
    | (SearchPathInner::FirstParty(search_path), FilePath::System(absolute_path))
```

The version of the match statement as a whole that merges the branches and that Clippy is happy with looks like this, which... seems kinda horrible to me (though it's admittedly less repetitive):

```rs
match (&*self.0, path) {
	(
	    SearchPathInner::Extra(search_path)
	    | SearchPathInner::FirstParty(search_path)
	    | SearchPathInner::StandardLibraryCustom(search_path)
	    | SearchPathInner::SitePackages(search_path)
	    | SearchPathInner::Editable(search_path),
	    FilePath::System(absolute_path),
	) => absolute_path
	    .strip_prefix(search_path)
	    .ok()
	    .map(|relative_path| ModulePath {
	        search_path: self.clone(),
	        relative_path: relative_path.as_utf8_path().to_path_buf(),
	    }),
	(
	    SearchPathInner::StandardLibraryVendored(search_path),
	    FilePath::Vendored(absolute_path),
	) => absolute_path
	    .strip_prefix(search_path)
	    .ok()
	    .map(|relative_path| ModulePath {
	        search_path: self.clone(),
	        relative_path: relative_path.as_utf8_path().to_path_buf(),
	    }),
	(
	    SearchPathInner::Extra(_)
	    | SearchPathInner::FirstParty(_)
	    | SearchPathInner::StandardLibraryCustom(_)
	    | SearchPathInner::SitePackages(_)
	    | SearchPathInner::Editable(_),
	    FilePath::Vendored(_),
	) => None,
	(SearchPathInner::StandardLibraryVendored(_), FilePath::System(_)) => None,
}
```

---

_@AlexWaygood reviewed on 2024-07-25 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:507 on 2024-07-25 18:10_

Never mind, I found a much nicer way of doing it!

---

_Merged by @AlexWaygood on 2024-07-25 18:29_

---

_Closed by @AlexWaygood on 2024-07-25 18:29_

---

_Branch deleted on 2024-07-25 18:29_

---

_@MichaReiser reviewed on 2024-07-25 18:44_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:576 on 2024-07-25 18:44_

It's the same as `&str` and `String`. It's preferred to take or return a `&str` because it is the more generic type. I think might even avoid an extra dereference because a `&SystemPathBuf` is a reference to a `SystemPathBuf` and you then need to dereference the value. `&SystemPath` is directly the reference to the value.

---

_@MichaReiser reviewed on 2024-07-25 18:45_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/path.rs`:73 on 2024-07-25 18:45_

Yeah, I think I would then go with the `kind` + `path` if we want to avoid that

---
