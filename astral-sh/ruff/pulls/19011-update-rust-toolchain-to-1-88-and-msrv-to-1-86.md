```yaml
number: 19011
title: Update Rust toolchain to 1.88 and MSRV to 1.86
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-1.88
created_at: 2025-06-28T15:04:07Z
updated_at: 2025-06-28T18:24:02Z
url: https://github.com/astral-sh/ruff/pull/19011
synced_at: 2026-01-10T18:39:09Z
```

# Update Rust toolchain to 1.88 and MSRV to 1.86

---

_Pull request opened by @MichaReiser on 2025-06-28 15:04_

## Summary

The most important new feature for us is that Rust now supports dyn upcasting. That allows us to remove the `Upcast` trait.

## Test Plan

cargo build


---

_Review requested from @carljm by @MichaReiser on 2025-06-28 15:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-28 15:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-28 15:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-28 15:04_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-28 15:04_

---

_Label `internal` added by @MichaReiser on 2025-06-28 15:04_

---

_Comment by @github-actions[bot] on 2025-06-28 15:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-28 15:17_

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

_@AlexWaygood approved on 2025-06-28 15:23_

---

_Comment by @AlexWaygood on 2025-06-28 15:23_

hmm, some windows path filtering stopped working?

---

_Comment by @MeGaGiGaGon on 2025-06-28 16:31_

Looks like it's because `<path>.path().canonicalize()` and `insta`'s path representation now disagree: The paths that are passed as a filter have a starting `\\?\` both on main and on this PR:
On main:
```
---- requires_python_pyproject_toml_above_with_tool stdout ----
[crates\ruff\tests\lint.rs:19:5] path.as_ref().to_str() = Some(
    "\\\\?\\C:\\Users\\GiGaGon\\AppData\\Local\\Temp\\.tmp8VZVtR\\foo\\test.py",
)
[crates\ruff\tests\lint.rs:19:5] path.as_ref().to_str() = Some(
    "\\\\?\\C:\\Users\\GiGaGon\\AppData\\Local\\Temp\\.tmp8VZVtR",
)
```

On this PR
```
---- requires_python_pyproject_toml_above_with_tool stdout ----
[crates\ruff\tests\lint.rs:19:5] path.as_ref().to_str() = Some(
    "\\\\?\\C:\\Users\\GiGaGon\\AppData\\Local\\Temp\\.tmpUGvoNc\\foo\\test.py",
)
[crates\ruff\tests\lint.rs:19:5] path.as_ref().to_str() = Some(
    "\\\\?\\C:\\Users\\GiGaGon\\AppData\\Local\\Temp\\.tmpUGvoNc",
)
```

But `insta`'s output doesn't have a starting `\\?\` as of this PR:
On main with filter removed
```snap
   78       │-  "[TMP]/foo",
   79       │-  "[TMP]/foo/src",
         78 │+  "\\?\C:\Users\GiGaGon\AppData\Local\Temp\.tmpAuA7Xq\foo",
         79 │+  "\\?\C:\Users\GiGaGon\AppData\Local\Temp\.tmpAuA7Xq\foo\src",
```

On this PR
```snap
   78       │-  "[TMP]/foo",
   79       │-  "[TMP]/foo/src",
         78 │+  "C:\Users\GiGaGon\AppData\Local\Temp\.tmpAQDo8w\foo",
         79 │+  "C:\Users\GiGaGon\AppData\Local\Temp\.tmpAQDo8w\foo\src",
```

And this is supported by the fact that all of the `lint` tests pass if I add a `.trim_start_matches(r"\\?\")` to `tempdir_filter`

---

_Comment by @MichaReiser on 2025-06-28 16:42_

Thanks for the analysis. The paths in your analysis look the same to me (ignoring the temp folder name) and I couldn't find any mention of changes to path handling in the Rust 1.88 changelog

---

_Comment by @MeGaGiGaGon on 2025-06-28 16:47_

Sorry if I was unclear on the issue (I need to get better at that)
On main: path passed to `tempdir_filter` starts with `\\?\`, path `insta` outputs starts with `\\?\`, regex matches and everything works
On this pr: path passed to `tempdir_filter` starts with `\\?\`, path `insta` outputs ***does not*** start with `\\?\`, regex fails to match and path is not replaced with `[TMP]` causing the test failure

---

_Comment by @ntBre on 2025-06-28 17:11_

This might have actually changed in 1.88? https://github.com/rust-lang/rust/pull/139683 I didn't see it in the release notes either, though.

I found this in the blame for `canonicalize` but it might not be related.

---

_Comment by @MichaReiser on 2025-06-28 17:45_

I think the culprint is https://github.com/rust-lang/rust/pull/138869 

`canonicalize` returns an UNC path on both branches. What's different is that we only see a UNC path in the spawned Ruff process on main but not with Rust 1.88. 

This is now a bit annoying to fix because downstream users might run the tests with both Rust 1.86 and 1.88. I think the easiest is to use dunce to avoid UNC paths in the first place

---

_Merged by @MichaReiser on 2025-06-28 18:24_

---

_Closed by @MichaReiser on 2025-06-28 18:24_

---

_Branch deleted on 2025-06-28 18:24_

---
