```yaml
number: 13757
title: Upgrade salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/upgrade-salsa-15-10
created_at: 2024-10-15T06:49:30Z
updated_at: 2024-10-15T11:20:35Z
url: https://github.com/astral-sh/ruff/pull/13757
synced_at: 2026-01-10T20:59:37Z
```

# Upgrade salsa

---

_Pull request opened by @MichaReiser on 2024-10-15 06:49_

This pulls in a MIRI fix and https://github.com/salsa-rs/salsa/pull/589 which allows us to look up interned structs by using a borrowed value (e.g. `StringLiteralType` can be resolved using a `&str` without having to allocate a `Box<str>` first.

---

_Review requested from @carljm by @MichaReiser on 2024-10-15 06:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-15 06:49_

---

_Label `red-knot` added by @MichaReiser on 2024-10-15 06:49_

---

_@dhruvmanila reviewed on 2024-10-15 06:54_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:795 on 2024-10-15 06:54_

I'm curious to know why this and other related changes are being made. I do prefer using `::from` instead of `.into` because it makes the conversion visible without peeking through the definition to know the final type.

---

_@MichaReiser reviewed on 2024-10-15 07:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:795 on 2024-10-15 07:05_

https://github.com/salsa-rs/salsa/pull/589 added a way to lookup interned types with borrowed values (rather than owned values). This made the `new` constructor  generic. I think I'm going to wait for https://github.com/salsa-rs/salsa/pull/594 because we can then change  `StringLiteralType::new` to be called with `"True"` and avoid allocating if the value has already been interned.

---

_@AlexWaygood reviewed on 2024-10-15 07:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:116 on 2024-10-15 07:08_

Does this work?

```suggestion
               self.elements.into_boxed_slice(),
```

---

_Comment by @github-actions[bot] on 2024-10-15 07:08_

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

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-15 09:18_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-15 09:18_

---

_@dhruvmanila approved on 2024-10-15 09:59_

---

_Review comment by @AlexWaygood on `Cargo.toml`:1 on 2024-10-15 10:48_

nit: there's lots of whitespace changes here that made the diff a bit harder to review. Not asking you to revert them as it would just be a waste of your time, just a comment for future PRs

---

_@AlexWaygood approved on 2024-10-15 10:48_

Nice! Love that we can get rid of all that `ModuleName` cloning

---

_@MichaReiser reviewed on 2024-10-15 11:01_

---

_Review comment by @MichaReiser on `Cargo.toml`:1 on 2024-10-15 11:01_

Sorry for that. RustRover loves to auto format files :(

---

_Merged by @MichaReiser on 2024-10-15 11:06_

---

_Closed by @MichaReiser on 2024-10-15 11:06_

---

_Branch deleted on 2024-10-15 11:06_

---
