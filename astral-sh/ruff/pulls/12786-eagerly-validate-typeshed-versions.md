```yaml
number: 12786
title: Eagerly validate typeshed versions
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: eagerly-resolve-typeshed-versions
created_at: 2024-08-09T13:39:44Z
updated_at: 2024-08-21T16:04:29Z
url: https://github.com/astral-sh/ruff/pull/12786
synced_at: 2026-01-12T15:55:42Z
```

# Eagerly validate typeshed versions

---

_@MichaReiser_

## Summary

This should be the last PR in the module resolver validation stack. 

It makes the typeshed version file parsing eagerly by moving it into `SearchPaths::from_settings`. This allows us to exit Red Knot early if the `VERSIONS` file is missing or incorrect. I added custom logic to the file watching to handle changes to the VERSIONS file (and added a test for it).

Handling changes to the `VERSIONS` file introduces some complexity because `Program::update_search_paths` takes a `SearchPathSettings` struct but we no longer know the `SearchPathSettings` in `apply_changes`. 

A possible solution to this problem is to expose a method `Program::reload_versions_file instead`, which would do the trick just fine. However, I disliked that it is very low-level. We may have other cases where we need to reload the search-path settings, and I want to avoid introducing special case methods for each of those. 

That's why I started looking into how we want to support configurations. What we need in `apply_changes` is access to what the user configured. The approach follows the same as Ruff's:

* `*Configuration`: The configuration that uses `Option` in most places to know whether the user provided a value or not. Configurations from different sources can be merged (e.g. CLI overrides the `pyproject.toml` configuration) 
* `*Settings`: A *resolved* configuration in the sense that Red Knot fills in default values and e.g. merges values from different configurations (resolves the `target-python` version).

This PR does not add support for loading or deserializing configurations. It just builds out some of the infrastructure. 


## Test Plan

`cargo test`

```bash
cargo run -q --bin red_knot -- --current-directory=../test -v --custom-typeshed-dir ./blaaa
INFO Target version: 3.8
Red Knot failed
  Cause: Invalid search path settings
  Cause: Failed to read the custom typeshed versions file '/home/micha/astral/test/blaaa/stdlib/VERSIONS': No such file or directory (os error 2)

```


---

_Label `red-knot` added by @MichaReiser on 2024-08-09 13:39_

---

_@MichaReiser reviewed on 2024-08-09 15:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:139 on 2024-08-09 15:24_

An alternative to storing `typeshed_versions` on `SearchPaths` would be to store the `TypeshedVersions` alongside the `SearchPath` (for the Custom or vendored stdlib variants). I don't really have an opinion on what's better. Let me know if you have  ;)

---

_Marked ready for review by @MichaReiser on 2024-08-09 15:26_

---

_Review requested from @carljm by @MichaReiser on 2024-08-09 15:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-09 15:26_

---

_Comment by @github-actions[bot] on 2024-08-09 15:39_

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

_Comment by @AlexWaygood on 2024-08-09 15:59_

If I understand correctly, this PR appears to treat the `VERSIONS` file as something that we can parse once upfront, and never parse again. That's true if we're using a vendored typeshed, but not if we're using a custom typeshed. The user might edit the `VERSIONS` file in between two queries; we need to be able to react to that and re-parse the `VERSIONS` file if our cached version is stale. That's why it's a Salsa query on `main`.

---

_Comment by @MichaReiser on 2024-08-09 16:09_

> If I understand correctly, this PR appears to treat the VERSIONS file as something that we can parse once upfront, and never parse again. That's true if we're using a vendored typeshed, but not if we're using a custom typeshed. The user might edit the VERSIONS file in between two queries; we need to be able to react to that and re-parse the VERSIONS file if our cached version is stale. That's why it's a Salsa query on main.

Hmm I missed this point. I have to add some manual file-watching to trigger a reload in this case. It also forces us to explicitly think about what should happen if parsing the VERSION files fails when we were able to parse it successfully before (in watch mode)

---

_Converted to draft by @MichaReiser on 2024-08-14 11:24_

---

_Comment by @codspeed-hq[bot] on 2024-08-14 11:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/eagerly-resolve-typeshed-versions)

### Merging #12786 will **degrade performances by 6.56%**

<sub>Comparing <code>eagerly-resolve-typeshed-versions</code> (ba1a840) with <code>main</code> (f873d2a)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/eagerly-resolve-typeshed-versions)._

### Benchmarks breakdown

|     | Benchmark | `main` | `eagerly-resolve-typeshed-versions` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 726.4 µs | 777.3 µs | -6.56% |


---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace.rs`:87 on 2024-08-14 17:17_

We may want to store the entire `WorkspaceSettings` on `Workspace` long term but it isn't clear to me if we actually want to do this. That's why I went with storing the absolute minimum

---

_@MichaReiser reviewed on 2024-08-14 17:17_

---

_@MichaReiser reviewed on 2024-08-14 17:18_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/settings.rs`:82 on 2024-08-14 17:18_

I'm a bit annoyed that we now have `SearchPathConfiguration`, `SearchPathSettings` and `SearchPaths` but I think the structs make sense:

* `SearchPathConfiguration`: The input as provided by the user
* `SearchPathSettings`: The resolved but not necessarily validated settings
* `SearchPaths`: The resolved and validated search paths.

---

_Marked ready for review by @MichaReiser on 2024-08-14 17:31_

---

_Converted to draft by @MichaReiser on 2024-08-14 17:47_

---

_Marked ready for review by @MichaReiser on 2024-08-15 09:32_

---

_@carljm reviewed on 2024-08-16 22:03_

I skimmed this and it looks reasonable to me, but I'd rather have @AlexWaygood take a look.

---

_Comment by @MichaReiser on 2024-08-20 19:26_

I plan to merge this tomorrow afternoon.

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:807 on 2024-08-21 13:47_

What does this comment mean? This test doesn't look like it involves `site-packages`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:303 on 2024-08-21 13:49_

```suggestion
    /// but `stdlib/VERSIONS` could not be read.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:315 on 2024-08-21 13:49_

```suggestion
    /// Failed to discover the site-packages for the configured virtual environment.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/path.rs`:334 on 2024-08-21 13:50_

```suggestion
                write!(f, "Failed to discover the site-packages directory: {error}")
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:297 on 2024-08-21 13:52_

Kinda weird to me that this is called `Typeshed`, when it only provides information about the `VERSIONS` file (doesn't provide any information about the typeshed stubs themselves). But not a huge deal, and I don't immediately have a better suggestion.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:708 on 2024-08-21 13:55_

The reason I put this in its own module (tiny though it is) was because it felt strange to me to import it from `resolver.rs` in `path.rs`, considering that `path.rs` is a "lower-level" module than `resolver.rs`. But maybe that's just me "thinking in Python" a bit too much, where it's really important to avoid cyclic dependencies between modules :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:697 on 2024-08-21 13:56_

These probably don't need to be `pub(crate)` anymore now that the whole module resolver is a submodule of `red_knot_python_semantic` (probably my oversight in the earlier PR)

```suggestion
    pub(super) db: &'db dyn Db,
    pub(super) target_version: PythonVersion,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/typeshed/versions.rs`:118 on 2024-08-21 13:57_

Does this need to be `pub(crate)` or could it be `pub(super)`?

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/db/changes.rs`:29 on 2024-08-21 13:58_

```suggestion
        // Changes to a custom stdlib path's VERSIONS
```

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace.rs`:329 on 2024-08-21 13:58_

```suggestion
                    "Found {} source files in package '{}'",
```

---

_@AlexWaygood approved on 2024-08-21 14:00_

Looks fantastic. Thanks for working on this, and sorry for the slow review!

---

_Comment by @MichaReiser on 2024-08-21 15:34_

Uff, rebasing this PR after so long is painful

---

_@MichaReiser reviewed on 2024-08-21 15:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:708 on 2024-08-21 15:40_

I think this is how it is intended in Rust where lower level modules have access to the parent but the inner modules are private to the parent.

---

_@MichaReiser reviewed on 2024-08-21 15:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:708 on 2024-08-21 15:42_

Ah I guess the real solution here would be to make `path` a submodule of `resolver`

---

_Merged by @MichaReiser on 2024-08-21 15:49_

---

_Closed by @MichaReiser on 2024-08-21 15:49_

---

_Branch deleted on 2024-08-21 15:49_

---
