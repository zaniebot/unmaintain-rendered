```yaml
number: 16147
title: "Move `red_knot_python_semantic::PythonVersion` to the `ruff_python_ast` crate"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/version-refactor
created_at: 2025-02-13T23:36:10Z
updated_at: 2025-02-14T17:48:10Z
url: https://github.com/astral-sh/ruff/pull/16147
synced_at: 2026-01-10T19:57:22Z
```

# Move `red_knot_python_semantic::PythonVersion` to the `ruff_python_ast` crate

---

_Pull request opened by @ntBre on 2025-02-13 23:36_

## Summary

This PR moves the `PythonVersion` struct from the `red_knot_python_semantic` crate to the `ruff_python_ast` crate so that it can be used more easily in the syntax error detection work. Compared to that [prototype](https://github.com/astral-sh/ruff/pull/16090/) these changes reduce us from 2 `PythonVersion` structs to 1.

This does not unify any of the `PythonVersion` *enums*, but I hope to make some progress on that in a follow-up.

## Test Plan

Existing tests, this should not change any external behavior.

---

_Review requested from @carljm by @ntBre on 2025-02-13 23:36_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-13 23:36_

---

_Review requested from @AlexWaygood by @ntBre on 2025-02-13 23:36_

---

_Review requested from @sharkdp by @ntBre on 2025-02-13 23:36_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-13 23:36_

---

_Comment by @github-actions[bot] on 2025-02-13 23:45_

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

_Label `internal` added by @ntBre on 2025-02-14 00:25_

---

_@MichaReiser reviewed on 2025-02-14 07:29_

---

_Review comment by @MichaReiser on `crates/red_knot/src/python_version.rs`:4 on 2025-02-14 07:29_

I think I'd like to keep this type because I don't want the parser to depend on clap. 

---

_@MichaReiser reviewed on 2025-02-14 07:31_

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/Cargo.toml`:25 on 2025-02-14 07:31_

I think I'd also prefer to keep the wasm wrapper to avoid having the parser depend on wasm-bindgen just for that.

---

_@MichaReiser reviewed on 2025-02-14 07:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python_version.rs`:5 on 2025-02-14 07:33_

I'd prefer to only have one python version in the parser crate. Having more than one seems confusing and unnecessary. Let's keep this in the linter if the linter isn't able to migrate to `PythonVersion` just yet. It makes it clear why the extra type exists and to which type we should migrate to

---

_@MichaReiser reviewed on 2025-02-14 07:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:84 on 2025-02-14 07:34_

I think it shouldn't be much effort to use `PythonVersion` here.

---

_@MichaReiser requested changes on 2025-02-14 07:35_

Thanks. 

I'd prefer to keep the API boundary Python version types used in the CLI and wasm because I don't want the parser to depend on clap or wasm bindgen (even optionally). It also allows us to add a new python version only to one tool but not the other. 

I'd also prefer not to move `PyVersion` to the parser. I find that confusing. We should keep this type in the linter crate if migrating the linter is too much effort. 

---

_@AlexWaygood reviewed on 2025-02-14 13:55_

I agree with unifying these enums/structs to some extent, but it does feel a bit odd to me that we'd have the common type in `ruff_python_parser`, since it's not really something specific to parsing Python code -- it's a general-purpose type that you could use for lots of things. `ruff_python_ast` might be a better fit -- or what about `ruff_python_stdlib`? If we put it there, we could use it in functions like this, which "should really" take a `PythonVersion` as their argument (it would make the signature much more strongly typed):

https://github.com/astral-sh/ruff/blob/63dd68e0edf606bc54afce091e543e5691552f97/crates/ruff_python_stdlib/src/builtins.rs#L222

I think the disadvantage might be that we'd have to declare an explicit dependency on `ruff_python_stdlib` from more other crates -- but I doubt that this would be a huge problem in practice, since both Ruff and red-knot already depend on it, and it's a pretty minimal crate that doesn't depend on any other crates in the workspace. Curious for @MichaReiser's thoughts...

---

_Comment by @MichaReiser on 2025-02-14 14:56_

I don't think it should be `ruff_python_stdlib` because I don't want the parser to depend on that crate. I wouldn't mind the ast crate.

---

_Comment by @AlexWaygood on 2025-02-14 14:58_

`ruff_python_ast` works for me -- we can leave the `ruff_python_stdlib` APIs as-is for now.

---

_Comment by @ntBre on 2025-02-14 15:03_

Sounds good, I already reverted everything except moving `red_knot_python_semantic::PythonVersion` to `ruff_python_parser`, so I will just move that to `ruff_python_ast` and then see if the formatter can use `PythonVersion`. I think I ran into `serde` issues with that yesterday because the (de)serialization needs to be different from red-knot's usage of `PythonVersion`, but I'll check again.

Thanks for the reviews!

---

_Comment by @ntBre on 2025-02-14 16:14_

`PythonVersion` is now in the `ast` crate, going to look at the formatter version now. I'll update the title and description if we just want to leave it here, though.

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/python_version.rs`:1 on 2025-02-14 16:49_

I suggest to reword one of the comments in this file slightly:

```diff
diff --git a/crates/ruff_python_ast/src/python_version.rs b/crates/ruff_python_ast/src/python_version.rs
index 727e68f0c..66745f8a4 100644
--- a/crates/ruff_python_ast/src/python_version.rs
+++ b/crates/ruff_python_ast/src/python_version.rs
@@ -2,8 +2,7 @@ use std::fmt;
 
 /// Representation of a Python version.
 ///
-/// Unlike the `TargetVersion` enums in the CLI crates,
-/// this does not necessarily represent a Python version that we actually support.
+/// N.B. This does not necessarily represent a Python version that we actually support.
 #[derive(Debug, Clone, Copy, Eq, PartialEq, Ord, PartialOrd, Hash)]
 pub struct PythonVersion {
     pub major: u8,
@@ -41,8 +40,7 @@ impl PythonVersion {
             PythonVersion::PY312,
             PythonVersion::PY313,
         ]
-        .iter()
-        .copied()
+        .into_iter()
     }
```

---

_@AlexWaygood approved on 2025-02-14 16:50_

---

_@MichaReiser approved on 2025-02-14 17:31_

---

_Comment by @MichaReiser on 2025-02-14 17:31_

Moving the type and doing the unification in separate PRs does make sense to me

---

_Comment by @ntBre on 2025-02-14 17:33_

Sounds good. I think I just started having some promising serde results, but it makes sense to open a separate PR. I'll push the change Alex recommended and then merge!

---

_Renamed from "Unify version structs and enums" to "Move `red_knot_python_semantic::PythonVersion` to the `ruff_python_ast` crate" by @ntBre on 2025-02-14 17:35_

---

_Merged by @ntBre on 2025-02-14 17:48_

---

_Closed by @ntBre on 2025-02-14 17:48_

---

_Branch deleted on 2025-02-14 17:48_

---
