```yaml
number: 12783
title: Eagerly validate search paths
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: eagerly-validate-search-paths
created_at: 2024-08-09T12:54:21Z
updated_at: 2024-08-12T07:56:39Z
url: https://github.com/astral-sh/ruff/pull/12783
synced_at: 2026-01-12T15:55:42Z
```

# Eagerly validate search paths

---

_@MichaReiser_

## Summary
This PR moves from a lazy search path validation and makes it eagerly so that Red Knot fails to start if the settings are incorrect.

The main change is that this PR removes `ModuleResolutionSettings` and instead stores the "resolved" `SearchPaths` directly on `Program`. Program gets a new `Program::from_settings(db, settings)` function that returns an error if any setting has an invalid value. 

## Test Plan

`cargo test`

```
cargo run -q --bin red_knot -- --current-directory=../test -v --extra-search-path ./blaaa
INFO Target version: py38
Red Knot failed
  Cause: Invalid search path settings
  Cause: ./blaaa does not point to a directory
```



---

_Label `red-knot` added by @MichaReiser on 2024-08-09 12:56_

---

_Comment by @github-actions[bot] on 2024-08-09 13:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-08-09 15:20_

---

_Review requested from @carljm by @MichaReiser on 2024-08-09 15:20_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-09 15:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:169 on 2024-08-09 15:21_

I haven't forgotten about the `unwrap`. I addressed it in the following PR. I think this PR even added a few more `as_system_path().unwrap()` calls.

---

_@MichaReiser reviewed on 2024-08-09 15:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:212 on 2024-08-09 15:32_

```suggestion
        // This code doesn't use an `IndexSet` because the key is the system path and not the search root.
        //
        // [`sys.path` at runtime]: https://docs.python.org/3/library/site.html#module-site
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:21 on 2024-08-09 15:38_

Couldn't we return `Result<Self, SearchPathvalidationError>` here? That error type was originally public. I made it private when merging the two crates to avoid some clippy complaints, but it would be fine to make it public again IMO

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:1 on 2024-08-09 15:41_

In #12684, I moved a lot of the settings-resolution logic out of this file and into a new `settings_resolution` submodule of the module resolver. I think that makes sense as it really has little to do with the rest of the logic into this file. It could also maybe go into `path.rs`, since you named the struct `SearchPaths`.

---

_@AlexWaygood approved on 2024-08-09 15:41_

Nice!

---

_@MichaReiser reviewed on 2024-08-09 15:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:21 on 2024-08-09 15:45_

`from_settings` could potentially return more errors than just `SearchPathvalidationError`. For example, we could error if we don't support the target version. That's why I used `anyhow` (and we have no upstream code that matches on the error anyway).

---

_@MichaReiser reviewed on 2024-08-09 15:46_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:1 on 2024-08-09 15:46_

I'll leave it where it is for now or I end up with ugly merge conflicts.

---

_@AlexWaygood reviewed on 2024-08-09 15:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:21 on 2024-08-09 15:50_

Currently it's impossible to have a target version here that we don't support because the `TargetVersion` enumeration only includes versions that we support, so clap's command-line validation should take care of that for us. But I see your point, thanks.

---

_Merged by @MichaReiser on 2024-08-12 07:46_

---

_Closed by @MichaReiser on 2024-08-12 07:47_

---

_Branch deleted on 2024-08-12 07:47_

---
