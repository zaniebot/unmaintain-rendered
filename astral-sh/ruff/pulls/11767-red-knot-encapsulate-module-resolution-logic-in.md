```yaml
number: 11767
title: "[red-knot] Encapsulate module resolution logic in `module.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: refactor-set-module-paths
created_at: 2024-06-05T22:43:42Z
updated_at: 2024-06-06T15:48:31Z
url: https://github.com/astral-sh/ruff/pull/11767
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] Encapsulate module resolution logic in `module.rs`

---

_@AlexWaygood_

This PR encapsulates all module-resolution logic in `module.rs`. Currently a `Vec` of search paths is passed into the routines in `module.rs`, but this opens the door to the search paths being passed in an incorrect order. The order of search paths should be maintained as an invariant via a common routine to avoid the possiblity of error.

`ResolvedSearchPathOrder::new()` implements the module resolution order given at https://typing.readthedocs.io/en/latest/spec/distributing.html#import-resolution-ordering, with some small differences (applying the changes proposed in https://discuss.python.org/t/pep-561-s-module-resolution-order-seems-incorrect/55048).

Work towards https://github.com/astral-sh/ruff/issues/11653

---

_Comment by @github-actions[bot] on 2024-06-06 10:55_

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

_Marked ready for review by @AlexWaygood on 2024-06-06 13:13_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-06 13:13_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-06 13:13_

---

_Label `red-knot` added by @MichaReiser on 2024-06-06 13:26_

---

_Comment by @AlexWaygood on 2024-06-06 13:26_

(Note that this PR essentially implements `--custom-typeshed-dir` internally, even though we don't yet support resolving to our vendored typeshed stubs.)

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:28 on 2024-06-06 13:26_

Haha, nooo xD. This is a huge setback on my quest to remove `is-macro`. I prefer writing the is methods manually. Rust-analyzer and other ideas often struggle with what is-macro generats, but that's a personal preference.  So, up to you if y ou want to keep it. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:49 on 2024-06-06 13:27_

Without having read the implementation. Passing an empty vec and `None` twice seems a bit weird. Is this temporary until we e.g. have typing-shed paths?

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:451 on 2024-06-06 13:30_

I think not using `chain` here would be easier to read
```suggestion
						let mut paths = extra_paths.into_iter().map(|path| ModuleSearchPath::new(path, ModuleSearchPathKind::Extra));
						
						paths.push(ModuleSearchPath::new(
                  workspace_root,
                  ModuleSearchPathKind::FirstParty,
              )));
              
              paths.extend(custom_typeshed.into_iter().map(|path| {
                  ModuleSearchPath::new(path, ModuleSearchPathKind::StandardLibrary)
              }));
                
              paths.extend(site_packages.into_iter().map(|path| {
                  ModuleSearchPath::new(path, ModuleSearchPathKind::SitePackagesThirdParty)
              }));

```

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:49 on 2024-06-06 13:59_

Reading more through the code. Ignore my comment. What's nice about `ResolvedSearchPathOrder` is that I think this can be modeled very easily with salsa when we have settings. 

All we need to do is to move it into a query that takes the type checker settings and it returns the resolved search paths. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:426 on 2024-06-06 14:01_

I would probably call this `ResolvedSearchPathOrder` because an instance of it doesn't implement an order, it's the ordered search paths. 

An alternative would be that `SearchPathOrder` sorts the paths: Given a list of `ModuleSearchPath`, sort the paths according to PEP-561 (using the `ModuleSearchPathKind)


---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:433 on 2024-06-06 14:04_

I think a slightly nicer API (requires a few more allocations but whathever, we build this once) is to use a builder:

```
ResolvedSearchPathBuilder::workspace(workspace_root)
    .extras(extras)
    .site_packages(site_packages)
    .typeshed(typeshed)
    .build()
```

where `ResolvedSearchPathBuilder` is 

```rust
struct ResolvedSearchPathBuilder {
    extras: Vec<PathBuf>,
    workspace_root: PathBuf,
    typeshed: Option<PathBuf>,
    site_packages: Option<PathBuf>
}
```

But I don't think it realy maters.

---

_@MichaReiser approved on 2024-06-06 14:04_

---

_@AlexWaygood reviewed on 2024-06-06 14:08_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:433 on 2024-06-06 14:08_

👍 I considered this, but here I think I prefer to force us to explicitly state whether we're passing in `site-packages` or whether we want to run in "isolated mode", ignoring all `site-packages` packages.

---

_@AlexWaygood reviewed on 2024-06-06 14:11_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:426 on 2024-06-06 14:11_

> I would probably call this `ResolvedSearchPathOrder`

Not sure I understand -- that's what it's already called

---

_@AlexWaygood reviewed on 2024-06-06 14:24_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:451 on 2024-06-06 14:24_

Hmm, to me it seems a fair bit more verbose but no more readable :/

```diff
--- a/crates/red_knot/src/module.rs
+++ b/crates/red_knot/src/module.rs
@@ -430,26 +430,40 @@ impl ResolvedSearchPathOrder {
         site_packages: Option<PathBuf>,
         custom_typeshed: Option<PathBuf>,
     ) -> Self {
-        Self(
+        let mut paths: Vec<ModuleSearchPath> = Vec::with_capacity(
+            extra_paths.len()
+                + 1
+                + usize::from(site_packages.is_some())
+                + usize::from(custom_typeshed.is_some()),
+        );
+
+        paths.extend(
             extra_paths
                 .into_iter()
-                .map(|path| ModuleSearchPath::new(path, ModuleSearchPathKind::Extra))
-                .chain(std::iter::once(ModuleSearchPath::new(
-                    workspace_root,
-                    ModuleSearchPathKind::FirstParty,
-                )))
-                // TODO fallback to vendored typeshed stubs if no custom typeshed directory is provided by the user
-                .chain(
-                    custom_typeshed.into_iter().map(|path| {
-                        ModuleSearchPath::new(path, ModuleSearchPathKind::StandardLibrary)
-                    }),
-                )
-                .chain(site_packages.into_iter().map(|path| {
-                    ModuleSearchPath::new(path, ModuleSearchPathKind::SitePackagesThirdParty)
-                }))
-                // TODO vendor typeshed's third-party stubs as well as the stdlib and fallback to them as a final step
-                .collect(),
-        )
+                .map(|path| ModuleSearchPath::new(path, ModuleSearchPathKind::Extra)),
+        );
+
+        paths.push(ModuleSearchPath::new(
+            workspace_root,
+            ModuleSearchPathKind::FirstParty,
+        ));
+
+        // TODO fallback to vendored typeshed stubs if no custom typeshed directory is provided by the user
+        paths.extend(
+            custom_typeshed
+                .into_iter()
+                .map(|path| ModuleSearchPath::new(path, ModuleSearchPathKind::StandardLibrary)),
+        );
+
+        paths.extend(
+            site_packages.into_iter().map(|path| {
+                ModuleSearchPath::new(path, ModuleSearchPathKind::SitePackagesThirdParty)
+            }),
+        );
+
+        // TODO vendor typeshed's third-party stubs as well as the stdlib and fallback to them as a final step
+
+        Self(paths)
     }

```

---

_@AlexWaygood reviewed on 2024-06-06 14:27_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:426 on 2024-06-06 14:27_

I think I see what you mean, though. I renamed it to `OrderedSearchPaths` in https://github.com/astral-sh/ruff/pull/11767/commits/4f0bcf5aa2dfd6a95533e9ec8d0fa87f100d3bd8

---

_Merged by @AlexWaygood on 2024-06-06 14:31_

---

_Closed by @AlexWaygood on 2024-06-06 14:31_

---

_Branch deleted on 2024-06-06 14:31_

---

_@MichaReiser reviewed on 2024-06-06 14:36_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:426 on 2024-06-06 14:36_

Oops sorry. That should have been *I probably **wouldn't** *. I didn't sleep well tonight and it is really showing. Sorry for this.

---

_@MichaReiser reviewed on 2024-06-06 14:37_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:451 on 2024-06-06 14:37_

Yeah, I probably would forgo the `with_capacity` optimisation which you also won't get with `std::iter::chain` because `ExactSizeIterator` isn't implemented for chained (because it could overflow). 



---

_@MichaReiser reviewed on 2024-06-06 14:38_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:28 on 2024-06-06 14:38_

Lol, I should have been more opinionated. It hurts that this is merged haha

---

_@AlexWaygood reviewed on 2024-06-06 14:39_

---

_Review comment by @AlexWaygood on `crates/red_knot/Cargo.toml`:28 on 2024-06-06 14:39_

Hahaha, I'm sorry. I've never had a problem with Rust-analyzer not being able to understand these generated methods locally!

---

_@AlexWaygood reviewed on 2024-06-06 14:40_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:451 on 2024-06-06 14:40_

> which you also won't get with `std::iter::chain` because `ExactSizeIterator` isn't implemented for chained (because it could overflow)

Ahh, I didn't realise that! But it makes sense now that you mention it.

I still kinda like the `chain()` personally, though 🤷

---

_@carljm reviewed on 2024-06-06 15:17_

---

_Review comment by @carljm on `crates/red_knot/Cargo.toml`:28 on 2024-06-06 15:17_

Sounds like what @MichaReiser really means by "rust-analyzer and other IDEs often struggle ..." is "RustRover, which I use, often struggles ..." ;)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:234 on 2024-06-06 15:19_

I find the word "dependency" here confusing. I wouldn't think of first-party code as a "dependency" -- the dependencies probably live in site-packages.

---

_Review comment by @carljm on `crates/red_knot/Cargo.toml`:28 on 2024-06-06 15:20_

(That said, I'm fine with avoiding this kind of thing -- it's also easier for humans to read explicit methods than a proc macro.)

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:237 on 2024-06-06 15:20_

the stdlib portion of typeshed

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:229 on 2024-06-06 15:21_

Worth a comment that these are listed here in priority order? And even a link to the relevant page of the typing spec (soon hopefully to be updated to match what we're doing here)?

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:433 on 2024-06-06 15:25_

I did almost comment on the call to `OrderedSearchPaths::new` above that it's hard to read/understand the call in code review, without IDE support, and without Python keyword arguments. Not sure how much this matters. Another way to address it would be taking in a config struct as argument instead of just positional args.

---

_@carljm reviewed on 2024-06-06 15:25_

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:451 on 2024-06-06 15:28_

I guess we could also `shrink_to_fit` the paths before returning, but probably doesn't matter given that few of these are ever created.

---

_@MichaReiser reviewed on 2024-06-06 15:29_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:433 on 2024-06-06 15:29_

I think we can ignore it for now. Ultimately what we'll do is to read the settings and take the fields from there (or fall back to other defaults).

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:675 on 2024-06-06 15:30_

I guess this is so when people point to a custom typeshed, they can just point to the root of the typeshed repo?

What about instead resolving this once upfront in the way we set our stdlib and typeshed-third-party search paths, so we don't have to do it on every resolution?

---

_@carljm reviewed on 2024-06-06 15:32_

---

_@AlexWaygood reviewed on 2024-06-06 15:40_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:234 on 2024-06-06 15:40_

I just carried over the pre-existing comment here, but yeah, agree that this isn't the best terminology. I'll fix it up in a followup.

---

_@AlexWaygood reviewed on 2024-06-06 15:44_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:675 on 2024-06-06 15:44_

> I guess this is so when people point to a custom typeshed, they can just point to the root of the typeshed repo?

Yup. That's what mypy and pyright do currently, and it's what I would expect a `--custom-typeshed-dir` argument to do.

> What about instead resolving this once upfront in the way we set our stdlib and typeshed-third-party search paths, so we don't have to do it on every resolution?

Could do! I decided to go this way so that calling `.path()` on the `ModuleSearchPath` instance would give you the path for `typeshed` rather than the path for `typeshed/stdlib`. That felt like it made the most sense here. As discussed elsewhere, I think we're going to have to do different things for each different kind of module search path anyway, so we're not going to be able to treat all paths in the resolved vector the same way even if this module search path pointed to `typeshed/stdlib` rather than just `typeshed`.

---

_@carljm reviewed on 2024-06-06 15:46_

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:675 on 2024-06-06 15:46_

To me it seems more useful if `.path()` on the `ModuleSearchPath` instance points to the actual path imports will be resolved from.

And agreed that `foo-stubs` packages mean special handling for resolutions in site-packages specifically, but in general the more work we can do upfront and the less work we can do on resolution of each package, the better.

---

_@AlexWaygood reviewed on 2024-06-06 15:48_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:675 on 2024-06-06 15:48_

Okay, no strong opinion from me on this; I can change this!

---
