```yaml
number: 12407
title: "[red-knot] Watch search paths"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: watch-search-paths
created_at: 2024-07-19T15:29:27Z
updated_at: 2024-07-24T07:53:23Z
url: https://github.com/astral-sh/ruff/pull/12407
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Watch search paths

---

_@MichaReiser_

## Summary

This PR extends the CLI to watch not only the workspace root but also all search paths. The watcher automatically registers new search paths or stops watching search paths when the program's `SearchPathSettings` change.

## Test Plan

Added integration tests.


---

_Comment by @github-actions[bot] on 2024-07-19 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_@MichaReiser reviewed on 2024-07-23 12:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1 on 2024-07-23 12:51_

I admit, adding `countme` fields to the different tracked structs isn't directly related to this PR but it seemed useful to have and it's a local enough change. But I can extract it if it is preferred.

---

_Label `red-knot` added by @MichaReiser on 2024-07-23 12:53_

---

_Marked ready for review by @MichaReiser on 2024-07-23 12:53_

---

_Review requested from @carljm by @MichaReiser on 2024-07-23 12:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-23 12:53_

---

_@MichaReiser reviewed on 2024-07-23 12:54_

---

_Review comment by @MichaReiser on `crates/red_knot/src/watch/workspace_watcher.rs`:17 on 2024-07-23 12:54_

I use a `Vec` here because this should almost always be empty and searching small vecs tends to be faster than hash maps, not that it matters much :laughing: 


---

_@AlexWaygood reviewed on 2024-07-23 12:54_

---

_Review comment by @AlexWaygood on `8.py`:1 on 2024-07-23 12:54_

🤨

---

_Comment by @codspeed-hq[bot] on 2024-07-23 12:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/watch-search-paths)

### Merging #12407 will **not alter performance**

<sub>Comparing <code>watch-search-paths</code> (0dcb095) with <code>watch-search-paths</code> (b396613)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_@MichaReiser reviewed on 2024-07-23 12:55_

---

_Review comment by @MichaReiser on `8.py`:1 on 2024-07-23 12:55_

Haha, I was just about to remove the file. I added it to test out https://github.com/astral-sh/ruff/issues/12475

---

_@AlexWaygood reviewed on 2024-07-23 12:56_

---

_Review comment by @AlexWaygood on `8.py`:1 on 2024-07-23 12:56_

Yeah I guessed 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/lib.rs`:21 on 2024-07-23 13:00_

nit: this is inconsistent with the import style I've used for the rest of this crate

```suggestion
use std::iter::FusedIterator;

use ruff_db::system::SystemPath;

pub use db::{Db, Jar};
pub use module::{Module, ModuleKind};
pub use module_name::ModuleName;
use resolver::{module_resolution_settings, SearchPathIterator};
pub use resolver::resolve_module;
pub use typeshed::{
    vendored_typeshed_stubs, TypeshedVersionsParseError, TypeshedVersionsParseErrorKind,
};
```

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/lib.rs`:50 on 2024-07-23 13:00_

Nice! I like how this means that we can continue to refactor the internals without it impacting the public API here

---

_@AlexWaygood reviewed on 2024-07-23 13:01_

The module-resolver crate bits look good (haven't looked at the other bits yet, but I will!)

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/lib.rs`:50 on 2024-07-23 13:12_

Yeah. I first exposed all the internal types as `pub` but that annoyed me. I then realized that I can just create my own iterator type and keep everything crate private :)

---

_@MichaReiser reviewed on 2024-07-23 13:12_

---

_@MichaReiser reviewed on 2024-07-23 13:13_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/lib.rs`:21 on 2024-07-23 13:13_

I think we should either deploy tooling to standardize use sorting or not be opinionated about it. It otherwise is guaranteed to get out of sync.

---

_@AlexWaygood reviewed on 2024-07-23 13:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/lib.rs`:21 on 2024-07-23 13:15_

Fair enough, I'll try to be less picky about it in the future :(

---

_@carljm reviewed on 2024-07-23 22:36_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/lib.rs`:21 on 2024-07-23 22:36_

FWIW I'd be happy to have tooling that standardizes it, but I haven't looked into what the options are.

---

_Review comment by @carljm on `crates/red_knot/src/workspace.rs`:248 on 2024-07-23 22:53_

very minor nit: to me the name `watch_paths` sounds like a verb name, suggesting this method actually watches some paths. maybe `paths_to_watch` or similar?

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:689 on 2024-07-23 22:59_

So we will have to ensure that however we update search path settings, we also call `watcher.update` in that code path?

---

_Review comment by @carljm on `crates/ruff_db/src/system/path.rs`:595 on 2024-07-23 23:04_

It's important that this always sorts shorter paths before longer ones, but perhaps that's obvious enough to not require comment?

---

_@carljm approved on 2024-07-23 23:05_

Nice!

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/lib.rs`:21 on 2024-07-24 07:26_

Rustfmt has an option for it but it isn't stabilized. https://rust-lang.github.io/rustfmt/?version=master&search=import#group_imports and it can't be enabled on non-nigthly rustfmt

```
Warning: can't set `group_imports = StdExternalCrate`, unstable features are only available in nightly channel.
Warning: can't set `unstable_features = true`, unstable features are only available in nightly channel.
```

---

_@MichaReiser reviewed on 2024-07-24 07:26_

---

_@MichaReiser reviewed on 2024-07-24 07:31_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:689 on 2024-07-24 07:31_

Not in `main`, but in tests yes. But this is a good point. Let me introduce a helper to update the search path settings that takes care of it.

---

_Merged by @MichaReiser on 2024-07-24 07:38_

---

_Closed by @MichaReiser on 2024-07-24 07:38_

---

_Branch deleted on 2024-07-24 07:38_

---
