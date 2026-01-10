```yaml
number: 20040
title: "[ty] Add search paths info to unresolved import diagnostics"
type: pull_request
state: merged
author: Renkai
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: show-resolve-paths
created_at: 2025-08-22T07:58:26Z
updated_at: 2025-08-26T15:01:22Z
url: https://github.com/astral-sh/ruff/pull/20040
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add search paths info to unresolved import diagnostics

---

_Pull request opened by @Renkai on 2025-08-22 07:58_

Fixes https://github.com/astral-sh/ty/issues/457

---

_Review requested from @carljm by @Renkai on 2025-08-22 07:58_

---

_Review requested from @AlexWaygood by @Renkai on 2025-08-22 07:58_

---

_Review requested from @sharkdp by @Renkai on 2025-08-22 07:58_

---

_Review requested from @dcreager by @Renkai on 2025-08-22 07:58_

---

_Converted to draft by @Renkai on 2025-08-22 07:58_

---

_Label `ty` added by @AlexWaygood on 2025-08-22 07:59_

---

_Comment by @github-actions[bot] on 2025-08-22 08:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Renamed from "[WIP] Add search paths info to unresolved import diagnostics" to "[ty] [WIP] Add search paths info to unresolved import diagnostics" by @AlexWaygood on 2025-08-22 08:03_

---

_Comment by @github-actions[bot] on 2025-08-22 08:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] [WIP] Add search paths info to unresolved import diagnostics" to "[ty] Add search paths info to unresolved import diagnostics" by @Renkai on 2025-08-22 08:25_

---

_Comment by @Renkai on 2025-08-22 08:28_

This solution called `search_paths` again to show the paths, not sure if carry the original results here is a better way, that way seems need changes in many places.

---

_Marked ready for review by @Renkai on 2025-08-22 08:28_

---

_Review requested from @MichaReiser by @Renkai on 2025-08-22 08:58_

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-22 11:34_

---

_Review request for @carljm removed by @carljm on 2025-08-22 18:41_

---

_Comment by @carljm on 2025-08-22 18:42_

Thanks for working on this!

@AlexWaygood can you take the lead on review here?

---

_Comment by @AlexWaygood on 2025-08-22 18:56_

Sure 

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 18:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/path.rs`:715 on 2025-08-23 19:02_

Ideally I feel like we'd be able to tell the user whether it was a CLI option or a config-file setting that caused us to look at an extra search path (or a custom stdlib search paths) during module resolution. But that would involve a fair bit of refactoring probably; I think it's best to leave it out of this PR.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5011 on 2025-08-23 19:03_

@BurntSushi -- thoughts on having a multiline diagnostic like this? Would it be better to render each line as a separate `info` diagnostic, or is this okay?

---

_@AlexWaygood approved on 2025-08-23 19:04_

Thanks, this looks great! I pushed a few minor tweaks to the PR.

It would be great to add a few more integration tests that have extra-paths set and/or an editable install.

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-08-25 16:41_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:77 on 2025-08-25 16:51_

Why insert this extra newline? (And also below.)

I guess the ones below are whatever, but this one should probably be removed to keep `crate` imports together in the same block.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:5011 on 2025-08-25 16:52_

Good catch. Yes, I think each item should be its own info sub-diagnostic, like what we do for overloads:

https://github.com/astral-sh/ruff/blob/e7237652a9048e5fa0e6ec002855d89f9554833b/crates/ty_python_semantic/src/types/call/bind.rs#L1809-L1819

---

_@BurntSushi requested changes on 2025-08-25 16:52_

---

_@Renkai reviewed on 2025-08-26 01:54_

---

_Review comment by @Renkai on `crates/ty_python_semantic/src/types/infer.rs`:77 on 2025-08-26 01:54_

Maybe some of my local formatter did this, I just removed it. Is it possible to include your role `keep crate imports together in the same block` in `cargo fmt` or something similar?

---

_@Renkai reviewed on 2025-08-26 02:09_

---

_Review comment by @Renkai on `crates/ty_python_semantic/src/types/infer.rs`:5011 on 2025-08-26 02:09_

updated

---

_Review requested from @BurntSushi by @Renkai on 2025-08-26 02:09_

---

_@BurntSushi reviewed on 2025-08-26 11:59_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:77 on 2025-08-26 11:59_

Do you mean [`imports_granularity`](https://rust-lang.github.io/rustfmt/?version=v1.8.0&search=#imports_granularity)? That isn't a stable rustfmt option.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/infer.rs`:4997 on 2025-08-26 12:01_

Why collect all the search paths into memory first? It seems a little gratuitous. You could do `let search_paths = search_paths(..).peekable();` and then `if search_paths.peek().is_some() {` and `for (index, path) in search_paths.enumerate() {` below.

---

_@BurntSushi approved on 2025-08-26 12:01_

LGTM with one minor nit and passing CI.

---

_@Renkai reviewed on 2025-08-26 13:51_

---

_Review comment by @Renkai on `crates/ty_python_semantic/src/types/infer.rs`:4997 on 2025-08-26 13:51_

updated by peekable. It seems requires more review to trigger next CI test. But the test looks fine on my computer now.

---

_Merged by @BurntSushi on 2025-08-26 15:01_

---

_Closed by @BurntSushi on 2025-08-26 15:01_

---

_Comment by @BurntSushi on 2025-08-26 15:01_

Thank you!!!

---
