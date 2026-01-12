```yaml
number: 16020
title: "Add `user_configuration_directory` to `System`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/system-user-configurationd-directory
created_at: 2025-02-07T13:11:52Z
updated_at: 2025-02-10T14:51:05Z
url: https://github.com/astral-sh/ruff/pull/16020
synced_at: 2026-01-12T15:55:53Z
```

# Add `user_configuration_directory` to `System`

---

_@MichaReiser_

## Summary

This PR adds a new `user_configuration_directory` method to `System`. We need it to resolve where to lookup a user-level `knot.toml` configuration file.
The method belongs to `System` because not all platforms have a convention of where to store such configuration files (e.g. wasm).


I refactored `TestSystem` to be a simple wrapper around an `Arc<dyn System...>` and use the `System.as_any` method instead to cast it down to an `InMemory` system. I also removed some `System` specific methods from `InMemoryFileSystem`, they don't belong there.

This PR removes the `os` feature as a default feature from `ruff_db`. Most crates depending on `ruff_db` don't need it because they only depend on `System` or only depend on `os` for testing. This was necessary to fix a compile error with `red_knot_wasm`

## Test Plan

I'll make use of the method in my next PR. So I guess we won't know if it works before then but I copied the code from Ruff/uv, so I have high confidence that it is correct.

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-02-07 13:12_

---

_Label `red-knot` added by @MichaReiser on 2025-02-07 13:12_

---

_Marked ready for review by @MichaReiser on 2025-02-07 13:12_

---

_Review requested from @carljm by @MichaReiser on 2025-02-07 13:12_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-07 13:12_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-07 13:12_

---

_Comment by @github-actions[bot] on 2025-02-07 14:24_

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

_@sharkdp reviewed on 2025-02-10 07:51_

---

_Review comment by @sharkdp on `crates/ruff_db/src/system/test.rs`:32 on 2025-02-10 07:51_

Minor suggestion:
```suggestion
    /// ## Panics
    /// If the underlying test system is not the in-memory system.
```

---

_@sharkdp reviewed on 2025-02-10 07:57_

---

_Review comment by @sharkdp on `crates/ruff_db/src/system/test.rs`:48 on 2025-02-10 07:57_

Maybe we could update the doc comment in this method as well (test system instead of test db?)

---

_@sharkdp approved on 2025-02-10 07:58_

Thank you

---

_Merged by @MichaReiser on 2025-02-10 14:50_

---

_Closed by @MichaReiser on 2025-02-10 14:50_

---

_Branch deleted on 2025-02-10 14:50_

---
