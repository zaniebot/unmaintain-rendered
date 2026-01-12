```yaml
number: 12684
title: "[red-knot] Eagerly validate search paths before constructing `Program`s"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/eager-search-validation
created_at: 2024-08-05T10:46:02Z
updated_at: 2025-01-31T15:05:30Z
url: https://github.com/astral-sh/ruff/pull/12684
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Eagerly validate search paths before constructing `Program`s

---

_@AlexWaygood_

## Summary

This is a prototype for _a_ way by which we could move settings resolution for search paths out of the module resolver. It may not be the _best_ way; I'm not wedded to it necessarily!

Moving settings resolution for search paths out of the module resolver enables us to validate that these settings are correct before constructing `Program` instances, which is desirable because `Program` instances should ideally store validated, normalized settings on them rather than unvalidated, unnormalized settings.

The way this PR works is that in `crates/ruff_db/src/program.rs`, new `SearchPathInner` and `SearchPath` types are added. These are extremely minimal, but in terms of the data they hold they have 1:1 correspondence with the `SearchPathInner` and `SearchPath` types in `crates/red_knot_module_resolver/path.rs`. Having public types in `ruff_db` that can be cheaply and easily converted to and from the private types in `red_knot_module_resolver` means that the `Program` struct can use a `SearchPath` type in its fields (representing a validated, normalized search path) without depending on anything from the module-resolver crate.

The module resolver, meanwhile, exposes a new public function for creating a `Program` instance from the raw, unvalidated search-path settings. This function uses internals of the module resolver to attempt to construct validated `red_knot_module_resolver::SearchPath`s, and then cheaply converts those search paths to `ruff_db::SearchPath`s in order to construct a `Program` instance.

I initially experimented with simply moving the `SearchPathInner` and `SearchPath` types out of the module resolver crate entirely and into `ruff_db`. While this is doable, it is quite painful, because validating search paths requires knowledge of the internals of the module resolver. For example: one of the validation steps we need to perform is to check that the file at `stdlib/VERSIONS` parses as a valid `VERSIONS` file if the user has given us a custom typeshed directory. Attempting to parse `VERSIONS` requires knowledge of the structure of the `VERSIONS` file, which in turn requires knowledge of the `ModuleName` type, etc. etc.

## Guide to reviewing

I would recommend looking at the changes to `ruff_db` first, then the changes to `red_knot_module_resolver`, and everything else after that.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-05 10:46_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-05 10:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-05 10:46_

---

_Comment by @github-actions[bot] on 2024-08-05 10:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-08-05 20:42_

> I initially experimented with simply moving the SearchPathInner and SearchPath types out of the module resolver crate entirely and into ruff_db. While this is doable, it is quite painful, because validating search paths requires knowledge of the internals of the module resolver.

It seems to me that a lot of the pain here arises from the fact that `Program` depends on module resolver, but module resolver must depend on `ruff_db` crate for the basic Salsa setup. But this seems like a constraint we've artificially placed on ourselves by having `Program` defined in `ruff_db`. As far as I can see nothing else in `ruff_db` depends on `Program`, so can't we just move `Program` out to `ruff_workspace` crate instead? Then `Program` can simply depend directly on the module resolver, and module resolver can depend on `ruff_db`, and we eliminate these issues.

---

_Comment by @AlexWaygood on 2024-08-05 20:44_

> As far as I can see nothing else in `ruff_db` depends on `Program`, so can't we just move `Program` out to `ruff_workspace` crate instead? Then `Program` can simply depend directly on the module resolver, and module resolver can depend on `ruff_db`, and we eliminate these issues.

That _sounds_ reasonable to me, but I'll defer to @MichaReiser on this :-)

---

_Comment by @MichaReiser on 2024-08-06 06:00_

>  Then Program can simply depend directly on the module resolver, and module resolver can depend on ruff_db, and we eliminate these issues.

I don't think this will work because the module resolver depends on `Program` to resolve the module resolution settings. I also suspect that some of the semantic model may require access to `Program`. 

I haven't looked at the specific implementation here and it may be worth thinking this through more carefully and re-reading through https://github.com/astral-sh/ruff/pull/12318.  

To me, the most natural place for `Program` is in the semantic model. That leaves the problem that the module resolver needs access to the search path settings and target version. We can either:

* Move the module resolver into the semantic model crate (as a sub module) and be done with it
* Expose a method on the module-resolver's `Db` trait to access the search paths settings. This leads to some boilerplate for all higher Db traits but would allow us to keep the two crates separated.

---

_Comment by @MichaReiser on 2024-08-07 10:18_

I plan to look at this once I start working on validation. Sorry for the delay. Let me know if you want feedback earlier 

---

_Comment by @AlexWaygood on 2024-08-07 10:19_

No worries; I don't see that as urgent. It's a pain that it touches so many files as merge conflicts keep cropping up, but it is what it is 😄

Please take your time!

---

_Comment by @MichaReiser on 2024-08-08 06:00_

I thought more about this and I'm leaning towards merging `red_knot_module_resolver` into `red_knot_python_semantic` because the module resolver is small(ish) and I don't see strong reasons that they have to be separate crates. Having both in a single crate greatly simplifies the problem because we can move `Program` into the semantic crate and be "done" with it.

@AlexWaygood what do you think of unifying the two crates? 

---

_Comment by @AlexWaygood on 2024-08-08 11:31_

Yeah I think combining the two crates could definitely make sense. I wasn't 100% sure it made sense for them to be separate crates to begin with. In theory it's nice to have a clean separation of concerns between the semantic-model crate and the module resolver but in practice it doesn't really work:
- The module resolver is only used by the semantic-model crate, so exactly what the return type of functions like `resolve_module()` is depends heavily on the implementation details of exactly what information the semantic-model crate needs from the module resolver
- There are already certain types, like `ModuleName`, which are included in (and exposed by) the module-resolver crate, but probably should really belong in the semantic-model crate. `ModuleName` isn't an implementation detail of the module resolver, it's a type that both the module resolver and the semantic model need to make use of. Arguably it would make more "sense" for it to be in the semantic-model crate, but it needs to be in the module-resolver crate currently betcause the module resolver needs access to it, and the module-resolver crate is lower level.

---

_Closed by @AlexWaygood on 2024-08-08 14:51_

---

_Branch deleted on 2025-01-31 15:05_

---
